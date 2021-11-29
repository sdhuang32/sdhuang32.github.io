---
title:  "用 OVS 及 Docker 在本地靜態網段中建立一個軟體定義的開發平台"
excerpt: "分享一個在靜態 (固定 IP) 網段中建立一個簡單開發平台的方法，
在你在本地開發容器化微服務時快速部署、串接、測試、並且與同個網段內的工程師同事共享服務。"
classes: wide
tags: 
  - Linux
  - OVS
  - Docker
  - Containers
  - Tools
date: 2020-08-24
last_modified_at: 2021-11-29
---

---
For English version of this post please see [HERE](/software-defined-local-dev-env-in-a-static-cidr-range-using-ovs-and-docker).

---

這邊想跟大家分享一個在靜態 (固定 IP) 網段中建立一個簡單開發平台的方法，
讓你可以在上面跑整個網段內都能存取的 Docker containers，
很適合用在本地開發微服務時快速部署、串接、測試、並且與同個網段內的工程師同事共享服務。
我個人曾經在工作中天天使用，覺得是很方便好用的本地開發技巧。

大致概念與步驟如下：
```
1. 在你的網段中一台實體機或虛擬機上全新安裝 CentOS 7。
2. 在 CentOS 7 中用 OVS (Open vSwitch) 設定網路橋接。
3. 在 CentOS 7 中啟動 container，並且借助網路橋接指定同網段的 IP。
```

在以下的圖中，我用一個範例網段跟一些範例 IP 來展示這個平台的大致架構：

![](https://raw.githubusercontent.com/sdhuang32/c7-ovs-docker-platform/master/ovs-docker-platform-structure.png)

## 前提條件
* 你 (及你的同事) 能夠 (從你的機器) 存取一個靜態網段 (此網段沒有 DHCP server 在分派 IP)，例如 `192.168.81.0/24`。
* 你有閒置的實體機或虛擬機平台 (例如 VMware ESXi 等) 位於這個網段可供你使用。

## 平台初始化步驟 (此為一次性操作)
1. 在你的實體機或虛擬機上安裝最新版 CentOS 7。
    * 如果你用的是 VMware ESXi 上的 VM，請先到所使用的 VM network 資訊頁，
    然後確定 `Security policy -> Allow promiscuous mode` 顯示為 `Yes` (我記得這個選項的中文是 `允許混合模式`)。 
    如果不是 Yes 的話，請到 `Edit settings -> Security -> Promiscuous mode` 然後選 `Accept`。
        * 這個功能預設是不啟用 (我當時是在 ESXi 6.5 上測試)。
        預設的效果是讓一台虛擬機的網卡只能接收到傳送給這張特定網卡的封包，
        用意是不希望虛擬機中的 process 可以看到所有在 ESXi 的虛擬交換器上飛來飛去的封包。
        [[1]][1] [[2]][2] [[3]][3]
        * 但是，由於我們要在虛擬機裡面跑 container，而這些 container 都會有自己的網卡跟 IP，
        卻是躲在 VM 裡面，網路流量要透過 VM 的網卡才能走進來，所以我們如果不啟用這個功能的話，
        就會沒法存取這些 VM 中的 container 提供的服務。
        * 也因此，如果你是使用其他虛擬化平台 (Hypervisor)，必須找到等同於此 `混合模式` 的功能並啟用它。

2. 在你安裝好的 CentOS 7 中執行以下指令：
```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/sdhuang32/c7-ovs-docker-platform/master/ovs-docker-host-setup.sh)"
```
**或者**是直接複製以下 script，然後貼到你的 CentOS 7 中並執行：
<script src="https://emgithub.com/embed.js?target=https%3A%2F%2Fgithub.com%2Fsdhuang32%2Fc7-ovs-docker-platform%2Fblob%2Fmaster%2Fovs-docker-host-setup.sh&style=github&showBorder=on&showLineNumbers=on&showFileMeta=on&showCopy=on"></script>
    * 這 script 會做這幾件事：
        * 安裝 openvswitch
        * 設定 ovs bridge `br-ext`
        * 安裝 docker-ce

到此我們的平台就準備好了，接下來就可以用這台 CentOS 7 來開發了!

## 運行與管理你的容器化服務
### 如何開 containers:
```bash
$ docker run --name nginx --hostname nginx --net=none -d nginx # 若使用自己的 image 請事先 build 好
# 執行以下指令前，先下 "route -n" 找到網段的 default gaeway，
# 並且下 "arp-scan -l" 或 "arp-scan -I <eth ID> -l" 找出網段中可用的 IP。
$ ovs-docker add-port br-ext eth0 nginx --ipaddress=192.168.81.102/24 --gateway=192.168.81.254
```
這樣就可以透過瀏覽器 `http://192.168.81.102/` 來存取你的 NginX 服務了，不需要仰賴任何的 port forwarding。

這麼做的好處在於用 IP 而非 port 來隔離不同的服務，所以我們可以用預設的存取方式、預設的 port 來存取我們的服務，
而不需要在 client side 改任何設定做任何客製化，服務部署完打了 IP 就能用，這比較直覺也比較貼近一般的使用情境，
在串接多個服務時也比較方便好管理。  
你可以重複套用上面的指令到其他的 container 上面，享受輕鬆互聯的快感 >.^

### 如何停止服務:
```bash
$ ovs-docker del-port br-ext eth0 nginx
$ docker stop nginx # 之後有需要的話可以 docker start nginx
$ docker rm nginx # 確定不再需要的時候，完全移除此 container 包括 non-volume/non-bind-mount 的資料
```

### 備註
需要注意的是，你的容器化服務都會仰賴單一 Linux 節點上的 Docker daemon，
所以這個方法當然不具備高可用性，同時也不適合用來管理大量 containers。
而且你需要用上面提的 `ovs-docker` 自己管理 container 用的虛擬網卡 (veth devices)，
有時候出問題時也許會需要用 `ip` 和 `ovs-vsctl` 等指令自行解決。結論是這個方法僅供本地開發測試使用，
不建議拿來開一些需要長期運作、大規模、或很重要的內部服務。


[1]: <https://ithelp.ithome.com.tw/questions/10191235>
[2]: <https://kb.vmware.com/s/article/1002934>
[3]: <https://kb.vmware.com/s/article/1004099>
