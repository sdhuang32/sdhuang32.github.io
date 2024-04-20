---
title:  "AWS CDK ssm.StringParameter.valueFromLookup() 應用場景與 Synthesis 流程分析"
excerpt: "跟大家分享 CDK 中 `ssm.StringParameter.valueFromLookup()` 可能造成的問題，synth 過程分析，以及解決方案"
classes: wide
tags:
  - AWS
  - CDK
  - Infrastructure as Code
date: 2021-11-23
last_modified_at: 2021-11-28
---

---
For English version of this post please see [HERE](/ssm-StringParameter-valueFromLookup-use-cases-and-internal-synth-flow).

---

這次要跟大家分享 CDK 中 `ssm.StringParameter.valueFromLookup()` 不太直覺的行為，以及它在不同使用場景中造成的問題以及解法。

## 情境介紹
我目前工作的環境由於公司內部安全性以及合規的要求，有相當嚴格的限制，
我所在的負責開發、維運服務的 team 不能直接使用 CDK 在部署時動態創建 IAM、KMS、security groups 等等資源。  
當我們最一開始要進行開發時，公司的 platform team 會給我們一個新的 AWS account，其中根據公司的政策自動配置好了
一個 VPC，幾個 subnets，相對應的 security groups，一些 KMS keys，還有一些 IAM roles，並且這些資源的 reference 
都已經記錄在 Systems Manager Parameter Store 之中，也就是像 VPC ID, security group IDs, IAM role ARNs, and KMS key ARNs 
這些，如果有額外需求，可以在一個受管的 git repo 裡面用 CloudFormation 自行添加需要的資源和對應的 SSM parameters，
然後提交給 security team 和 networking team 來審查，一旦通過並且成功部署以後，**我們就可以在自己的 CDK app 中用 
`ssm.StringParameter.valueFromLookup()` 從 Parameter Store 查詢需要的資源 IDs 或 ARNs，再 import 這些資源來使用。** 

## 觀察到的問題

以下這些問題和後面的解法都可以在 CDK v1.133.0 中重現並且驗證。

##### 使用 role ARN 來 import IAM role

```typescript
import * as cdk from '@aws-cdk/core';
import * as ssm from '@aws-cdk/aws-ssm';
import * as iam from '@aws-cdk/aws-iam';

const roleArn = ssm.StringParameter.valueFromLookup(this, "/param/testRoleArn");
const role = iam.Role.fromRoleArn(this, "role", roleArn);
```
我們可以在第一次跑 `cdk synth` 的時候看到以下錯誤訊息:
```
ARNs must start with "arn:" and have at least 6 components: dummy-value-for-/param/testRoleArn
```

##### 使用 key ARN 來 import KMS key

```typescript
import * as cdk from '@aws-cdk/core';
import * as ssm from '@aws-cdk/aws-ssm';
import * as iam from '@aws-cdk/aws-kms';

const keyArn = ssm.StringParameter.valueFromLookup(this, "/param/keyArn");
const key = kms.Key.fromKeyArn(this, "key", keyArn);
```
我們可以在第一次跑 `cdk synth` 的時候看到以下錯誤訊息:
```
ARNs must start with "arn:" and have at least 6 components: dummy-value-for-/param/keyArn
```

##### 用 parameter 給的字串作為新建 S3 bucket 時的 bucket name

```typescript
import * as cdk from '@aws-cdk/core';
import * as ssm from '@aws-cdk/aws-ssm';
import * as s3 from '@aws-cdk/aws-s3';

const bucketName = ssm.StringParameter.valueFromLookup(this, "/param/bucketName");
const testBucket = new s3.Bucket(this, "testBucket", {
  bucketName: bucketName
});
```
我們可以在第一次跑 `cdk synth` 的時候看到以下錯誤訊息:
```
Invalid S3 bucket name (value: dummy-value-for-/param/bucketName)
Bucket name must only contain lowercase characters and the symbols, period (.) and dash (-) (offset: 16)
```

在研究這些問題的過程中我也看到許多朋友遇到一樣的問題:
- [CDK issue #7477][7477]
- [CDK issue #8699][8699]
- [CDK issue #7051][7051]
- [CDK issue #6153][6153]
- [CDK issue #9138][9138]

## 根本原因

根據我對於 [CDK app lifecycle][2] 的理解，我們從 command line 下 `cdk synth` 的時候，
CDK 背後會依序進行以下動作 (若有錯誤歡迎留言指正!)：
1. Construction - 根據你定義的 constructs 在記憶體中建立物件，並互相連結在一起，
   其中也包括執行原生 Typescript 的敘述 (例如 `console.log()`)。
2. Preparation
3. Validation - 檢查是否出現一些會導致部署失敗的配置錯誤，例如 circular dependency，
   目標是確保最後產出的 CloudFormation template 可以順利被執行。
4. Synthesis 
    1. 如果使用了一些需要從 context 取得正確數值的 [methods][1] (e.g. VPC info, SSM parameters) 
    但是在 construction phase (步驟 1) 時沒有找到對應的值，那麼就從你指定的 AWS account 之中去抓，
    然後**從 construction phase (步驟 1) 重新開始整個 synth 流程**。
    如果需要的值都已從 context 取得，則繼續進行下一步。
    2. 把記憶體中的 construct 物件都轉換為最終的 *cloud assembly*，也就是 CloudFormation template 和相關產物。
    3. 若有必要 (例如使用了 [Raw overrides][3] 功能)，則強制修改 CloudFormation template 中的部分欄位。

當前 `ssm.StringParameter.valueFromLookup()` 的實作會在 **construction time (步驟 1)** 到 [context][1] 
用指定的 parameter key 查詢你要的值，如果找不到 (第一次執行 `cdk synth`，context 裡面空空如也的時候)，
就會留到 **synthesis time (步驟 4)** 的時候再真的到你的 AWS account 裡面去抓。

問題就在於，在它能夠走到 synthesis 這一步然後抓到正確的值並開啟第二輪 synth 之前，
這 method 會先回傳一個暫時性字串 `dummy-value-for-${parameterName}`，
而這個字串很有可能無法通過一些 constructs 或 method 的字串參數格式檢查，進而造成在 construction phase 就失敗。

## 應用場景及解決方案

##### 使用 role ARN 來 import IAM role

`iam.Role.fromRoleArn()` 這個 method 會在呼叫 [Arn.split()][4] 的過程中驗證你塞給它的 ARN 字串格式，
這就是我們前面看到 `ARNs must start with "arn:" and have at least 6 components` 錯誤訊息的原因，
同時這個邏輯應該是套用在了大部分或所有 `fromXXXArn()` methods 裡面，所以這類的 methods 都會碰到相同問題。

第一個解法 (感謝 [issue #8699][8699] 中的 Thomas 提供) 是先填入一個暫時性的合法 ARN 字串，
讓 `fromRoleArn` 可以正確地通過格式檢查並且讓整個 construction phase 可以完成，
這樣就能順利進展到 synthesis phase 抓取真正的 parameter value 並且啟動第二輪 synth，
`valueFromLookup` 就可以從 context 得到正確的 parameter (role ARN) 並且送交 `fromRoleArn` 處理。

```typescript
import * as cdk from '@aws-cdk/core';
import * as ssm from '@aws-cdk/aws-ssm';
import * as iam from '@aws-cdk/aws-iam';

let roleArn = ssm.StringParameter.valueFromLookup(this, "/param/testRoleArn");
if (roleArn.includes('dummy-value')) {
  roleArn = 'arn:aws:service:eu-central-1:123456789012:entity/dummy-value';
}
const role = iam.Role.fromRoleArn(this, "role", roleArn);
```

第二個解法則是我最近想到的，使用 [Lazy values][5] 來解決這個問題。
精確地說，在這個例子中我用了CDK core module 中的 [Lazy.string()][6] method，
它可以 encode 你的 variable 變成一個 token，並且延後到 synthesis time 再來幫你算出它真正的值。
非常適合用在這種，當你的某個變數依賴於其他 synthesis time 才會確定的數值的時候。

```typescript
import * as cdk from '@aws-cdk/core';
import * as ssm from '@aws-cdk/aws-ssm';
import * as iam from '@aws-cdk/aws-iam';

const roleArn = ssm.StringParameter.valueFromLookup(this, "/param/testRoleArn");
const role = iam.Role.fromRoleArn(this, "role", cdk.Lazy.string({ produce: () => roleArn }));
```

解法二會更漂亮也更一般化一點，因為它可以使用在其他情境中，除了其他 `fromXXXArn()` 的場景外，
還有例如下面會提到的 S3 bucket creation 範例。
同時這個用法其實被大量地使用在[原生 CDK source code][7] 之中，用來把一些值延後到 synthesis time 再產生，
**不過前提是要確保吃到這些 Lazy value 的 method 有實做處理 token 的部分!**

##### 用 parameter 給的字串作為新建 S3 bucket 時的 bucket name

`s3.Bucket` 的 constructor 是支援 [用 token 作為 bucket name][8] 的，所以我們可以套用上面的解法二到這個情境。

```typescript
import * as cdk from '@aws-cdk/core';
import * as ssm from '@aws-cdk/aws-ssm';
import * as s3 from '@aws-cdk/aws-s3';

const bucketName = ssm.StringParameter.valueFromLookup(this, "/param/bucketName");
const testBucket = new s3.Bucket(this, "testBucket", {
  bucketName: cdk.Lazy.string({ produce: () => bucketName })
});
```

##### 用 VPC ID 來 Import 一個現有的 VPC

這邊我想提另一個不太一樣的使用案例，我們可以從中觀察到不同的行為。

當我們使用 `ec2.Vpc.fromLookup()` 來 import 一個 VPC 的時候，
只要直接把 `ssm.StringParameter.valueFromLookup()` 的回傳值丟過去就行了，無需做任何處理，如下。
```typescript
import * as cdk from '@aws-cdk/core';
import * as ssm from '@aws-cdk/aws-ssm';
import * as ec2 from '@aws-cdk/aws-ec2';

const vpcId = ssm.StringParameter.valueFromLookup(this, "/param/vpcId");
const vpc = ec2.Vpc.fromLookup(this, "vpc", {
  vpcId: vpcId
});
```

這是因為 `ec2.Vpc.fromLookup()` 不會檢查 `vpcId` 這個輸入參數的字串格式，
所以不會導致 construction phase 因此失敗，也就能讓 synth 順利走到第二輪的緣故。

但是，這個 method 要求你輸入的 `vpcId` 必須是一個 concrete string，也就是不能給 token，
如果我們套用上面的解法來如法炮製 `vpcId: cdk.Lazy.string({produce: () => vpcId })` 的話，
將會導致錯誤 [`All arguments to Vpc.fromLookup() must be concrete (no Tokens)`][9]。

從這個例子中我們也可以看到，若讓 `ssm.StringParameter.valueFromLookup()` 回傳一個 token 而非純字串，
並不是一個解決這類問題的好辦法，因為你永遠不知道接下來要使用 parameter 的 method 會對輸入字串做怎樣的格式檢查，
但我們可以存任何種類、任何用途的字串在一個 SSM parameter 裡面，
所以很難要求一個從 `valueFromLookup()` 內部搞定的大一統解法。
除非 (我心目中最理想的解) CDK team 做一個大重構然後把它重新設計成在 construction time 就去 AWS account 抓 parameter value，
那才能夠真正一勞永逸解決這些問題。

就現階段來說，當前最好的解法恐怕還是得先搞清楚自己的使用情境，然後看一下你打算用來接 parameter 的 method 是如何實作的 
(有沒有格式檢查、能不能吃 token 等等)，然後再來決定是否需要在傳 parameter 當參數之前先做點前處理，以及用什麼方式前處理。


[1]: <https://docs.aws.amazon.com/cdk/latest/guide/context.html#context_methods>
[2]: <https://docs.aws.amazon.com/cdk/latest/guide/apps.html#lifecycle>
[3]: <https://docs.aws.amazon.com/cdk/latest/guide/cfn_layer.html#cfn_layer_raw>
[4]: <https://github.com/aws/aws-cdk/blob/v1.133.0/packages/%40aws-cdk/core/lib/arn.ts#L207>
[5]: <https://docs.aws.amazon.com/cdk/latest/guide/tokens.html#tokens_lazy>
[6]: <https://docs.aws.amazon.com/cdk/api/latest/docs/@aws-cdk_core.Lazy.html#static-stringproducer-options>
[7]: <https://github.com/aws/aws-cdk/search?q=%22Lazy.string%28%22>
[8]: <https://github.com/aws/aws-cdk/blob/v1.133.0/packages/%40aws-cdk/aws-s3/lib/bucket.ts#L1446>
[9]: <https://github.com/aws/aws-cdk/blob/v1.133.0/packages/@aws-cdk/aws-ec2/lib/vpc.ts#L1099>
[7477]: <https://github.com/aws/aws-cdk/issues/7477>
[8699]: <https://github.com/aws/aws-cdk/issues/8699>
[7051]: <https://github.com/aws/aws-cdk/issues/7051>
[6153]: <https://github.com/aws/aws-cdk/issues/6153>
[9138]: <https://github.com/aws/aws-cdk/issues/9138>

