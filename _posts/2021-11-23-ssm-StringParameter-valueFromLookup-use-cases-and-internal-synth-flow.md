---
title:  "AWS CDK ssm.StringParameter.valueFromLookup() use cases and internal synthesis flow"
excerpt: "Here is a handy way to establish a simple development environment within your private LAN, 
which enables you to quickly and easily develop, test, and troubleshoot your containerized applications."
classes: wide
tags: 
  - AWS
  - CDK
  - Infrastructure as Code
date: 2021-11-23
last_modified_at: 2021-11-23
---

---
這篇文章的繁體中文版本請看[這裡](/zh-tw/ssm-StringParameter-valueFromLookup-use-cases-and-internal-synth-flow)。

---

In this post I'm gonna share the interesting behavior of CDK `ssm.StringParameter.valueFromLookup()` method, 
the issues it might cause, and the solutions to them.

## Background
I'm currently working in a highly restrictive environment. 
Due to security and compliance requirements, we engineers responsible for applications 
are not allowed to create IAM, KMS, security groups, etc. resources *at deployment time* 
(i.e. we cannot create these resources in our CDK apps or CloudFormation templates along with the applications).

When we want a new workspace, the platform team of the company will give us a new AWS account with a VPC, 
some subnets, some security groups, some KMS keys, and some IAM roles, 
all things automatically pre-provisioned in line with the company's policies, 
and the references to these resources are recorded in the Systems Manager Parameter Store, 
such as the VPC ID, security group IDs, IAM role ARNs, and KMS key ARNs.  
If we want to add more, we submit our own customized IAM roles and so on with associated SSM parameters 
using CloudFormation in a separate, managed git repo, and seek approval from the security team and networking team. 
After approval and things get deployed, **in a CDK app we look up the references (IDs, ARNs) of the resources 
we need from Parameter Store using `ssm.StringParameter.valueFromLookup()`, and import them to use**.

## Issues found

The following issues and later the solutions are verified at CDK v1.133.0.

##### Import an IAM role using the role ARN

```typescript
import * as cdk from '@aws-cdk/core';
import * as ssm from '@aws-cdk/aws-ssm';
import * as iam from '@aws-cdk/aws-iam';

const roleArn = ssm.StringParameter.valueFromLookup(this, "/param/testRoleArn");
const role = iam.Role.fromRoleArn(this, "role", roleArn);
```
At first time running `cdk synth` you will get the following error message:
```
ARNs must start with "arn:" and have at least 6 components: dummy-value-for-/param/testRoleArn
```

##### Import an KMS key using the key ARN

```typescript
import * as cdk from '@aws-cdk/core';
import * as ssm from '@aws-cdk/aws-ssm';
import * as iam from '@aws-cdk/aws-kms';

const keyArn = ssm.StringParameter.valueFromLookup(this, "/param/keyArn");
const key = kms.Key.fromKeyArn(this, "key", keyArn);
```
At first time running `cdk synth` you will get the following error message:
```
ARNs must start with "arn:" and have at least 6 components: dummy-value-for-/param/keyArn
```

##### Create an S3 bucket with the name from a parameter
```typescript
import * as cdk from '@aws-cdk/core';
import * as ssm from '@aws-cdk/aws-ssm';
import * as s3 from '@aws-cdk/aws-s3';

const bucketName = ssm.StringParameter.valueFromLookup(this, "/param/bucketName");
const testBucket = new s3.Bucket(this, "testBucket", {
  bucketName: bucketName
});
```
At first time running `cdk synth` you will get the following error message:
```
Invalid S3 bucket name (value: dummy-value-for-/param/bucketName)
Bucket name must only contain lowercase characters and the symbols, period (.) and dash (-) (offset: 16)
```

I also found that many other friends had the same issues:
- [CDK issue #7477][7477]
- [CDK issue #8699][8699]
- [CDK issue #7051][7051]
- [CDK issue #6153][6153]
- [CDK issue #9138][9138]

## Root cause

When you call `cdk synth` from the command line, according to my understanding of 
[CDK app lifecycle][2]
it will do the following things behind the scenes chronologically:  
(if you see something wrong, please comment and let me know ;) )
1. Construction - instantiate all constructs you defined along with native typescript statements such as `console.log()`, 
   create objects of these resources, and link them together.
2. Preparation
3. Validation - check, for example, if there is circular dependency between resources, etc. to make sure the resources you defined can be correctly deployed.
4. Synthesis 
    1. if there is some context required by [these methods][1] (e.g. VPC info, SSM parameters) but missing, 
    fetch them from your AWS account, and then **start over the synth process again from the construction phase (step 1)**. 
    If not, continue to the next step.
    2. transform the in-memory construct objects into the resulting *cloud assembly*, namely, 
    CloudFormation template and other stuff.
    3. update some properties of the generated CloudFormation template if necessary 
    (e.g. when we use a [Raw overrides][3] in the code)

Current implementation of `ssm.StringParameter.valueFromLookup()` will get the parameter value
by looking up the parameter key in the current [context][1] **at construction time (step 1)**. 
If the parameter is not available yet (the first time you call `cdk synth` when it's empty in the context) 
it will be fetched from your AWS account **at synthesis time (step 4)**.

However, before synthesis getting the correct parameter and starting the second synth, 
the method will return a string `dummy-value-for-${parameterName}` and this string might not pass 
the string parameter validation of some other constructs and functions in the same construction phase, 
and cause the first synth to fail.


## Use cases and solutions

##### Import an IAM role using the role ARN

The `iam.Role.fromRoleArn()` method validates the format of input ARN string by calling [Arn.split()][4], 
and that's why we see the `ARNs must start with "arn:" and have at least 6 components` error, 
and I believe this logic applies to pretty much all `fromXXXArn()` methods of other CDK modules.

Solution one (thanks to Thomas in [issue #8699][8699]) is to fill a placeholder for `fromRoleArn` 
to correctly parse and finish the construction phase without error, 
and this placeholder will be replaced because the synth process will start over again 
and `valueFromLookup` will get the correct parameter value in the context after all.

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

Solution two which I thought of recently, is to use [Lazy values][5]. 
To be specific, here we use the [Lazy.string()][6] of the CDK core module, which encodes your variable as a token 
and defer the calculation of the string value to synthesis time.

```typescript
import * as cdk from '@aws-cdk/core';
import * as ssm from '@aws-cdk/aws-ssm';
import * as iam from '@aws-cdk/aws-iam';

const roleArn = ssm.StringParameter.valueFromLookup(this, "/param/testRoleArn");
const role = iam.Role.fromRoleArn(this, "role", cdk.Lazy.string({ produce: () => roleArn }));
```

Solution two is bit more elegant and general because it can apply to many other use cases 
besides `fromXXXArn()` methods (for example the following S3 use case), 
and is in fact used in [many places of internal CDK source][7] to get values rendered at synthesis time, 
**as long as the method you throw a lazy value to can handle tokens**.

##### Create an S3 bucket with the name from a parameter

The constructor of `s3.Bucket` supports [tokens as a bucket name][8] so we can apply the solution two above to this case.

```typescript
import * as cdk from '@aws-cdk/core';
import * as ssm from '@aws-cdk/aws-ssm';
import * as s3 from '@aws-cdk/aws-s3';

const bucketName = ssm.StringParameter.valueFromLookup(this, "/param/bucketName");
const testBucket = new s3.Bucket(this, "testBucket", {
  bucketName: cdk.Lazy.string({ produce: () => bucketName })
});
```

##### Import an existing VPC using the VPC ID

Here I'm gonna mention another use case and we can see the subtle difference.

When importing a VPC using `ec2.Vpc.fromLookup()`, just give the return value of the 
`ssm.StringParameter.valueFromLookup()` and it works.
```typescript
import * as cdk from '@aws-cdk/core';
import * as ssm from '@aws-cdk/aws-ssm';
import * as ec2 from '@aws-cdk/aws-ec2';

const vpcId = ssm.StringParameter.valueFromLookup(this, "/param/vpcId");
const vpc = ec2.Vpc.fromLookup(this, "vpc", {
  vpcId: vpcId
});
```

This is because `ec2.Vpc.fromLookup()` does not check the format of `vpcId` input parameter, 
so it won't cause the construction phase to fail.

However, this method demands your input `vpcId` to be a concrete string and not a token, so the statement
`vpcId: cdk.Lazy.string({produce: () => vpcId })` will result in the error 
[`All arguments to Vpc.fromLookup() must be concrete (no Tokens)`][9]. 
This is also why making the `ssm.StringParameter.valueFromLookup()` to return a token instead of a plain string 
is not a good idea, because you never know how the callee will deal with the input parameter.

You can store any kind of strings, for different purposes in a SSM parameter, 
so it's hard to demand a universal fix for every use cases from inside the `valueFromLookup()` method,
unless (the ideal fix in my mind) the CDK team does a big re-design and make it fetch the parameters at construction time.

At the moment the best way is to understand your use case and have a look at the source code of the method 
you're gonna pass your parameter to, and decide what approach you need to pre-process your parameter.


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

