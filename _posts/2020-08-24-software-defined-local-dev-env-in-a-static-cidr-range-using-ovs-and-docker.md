---
title:  "Software-defined local development platform in a static CIDR range using OVS and Docker"
excerpt: "Here is a handy way to establish a simple development environment within your private LAN, 
which enables you to quickly and easily develop, test, and troubleshoot your containerized applications."
classes: wide
tags: 
  - Linux
  - OVS
  - Docker
  - Containers
  - Tools
date: 2020-08-24
last_modified_at: 2021-11-19
---

---
這篇文章的繁體中文版本請看[這裡](/zh-tw/software-defined-local-dev-env-in-a-static-cidr-range-using-ovs-and-docker)。

---

In this post I'm gonna share a handy way of creating a simple development platform on-premises within your static 
private LAN, which I used in my every single working day, enabling you to run internally public containers, 
so you can quickly develop, test, and troubleshoot your microservices applications together with other colleague engineers.

The concept is as follows.
```
1. Fresh install a CentOS 7 Linux on a physical or virtualized server within your network.
2. Setup the virtual switch inside the CentOS 7 using OVS (Open vSwitch) as the network bridge.
3. Launch application containers with IPs in the same network.
```

The following is the diagram with an example CIDR range and IPs, showing the structure of this simple platform.

![](https://raw.githubusercontent.com/sdhuang32/c7-ovs-docker-platform/master/ovs-docker-platform-structure.png)

## Prerequisites
* You have access to a static network (there is no DHCP server distributing IPs), for example, `192.168.81.0/24`.
* You have a physical machine or hypervisor node (e.g. VMware ESXi) inside this network.

## Setup steps (one-time operation)
1. Install latest CentOS 7 on your physical machine or VM.
    * If you start a VM on VMware ESXi, check the info page of the VM network that your VM is using, 
    and make sure `Security policy -> Allow promiscuous mode` shows `Yes`. 
    If not, go to `Edit settings -> Security -> Promiscuous mode` and choose `Accept`.
        * The Promiscuous mode is disabled by default (I tested on ESXi 6.5). 
        This will allow a network interface of a guest VM to only receive traffic dedicatedly sent to it. 
        The original intention is that it's not encouraged for a guest VM to see all traffic flowing through 
        a vSwitch of the hypervisor. [Reference 1][1] [Reference 2][2]
        * However, we are going to run containers inside a guest VM, 
        and these containers will have their own network interfaces and IPs, 
        kind of like *hidden* behind the guest VM network interface, 
        so if we don't enable this option, we will not be able to access the services hosted in the containers.
        * Hence if you are using other hypervisors, 
        you have to find out the equivalent option of `Promiscuous mode` and enable it.

2. Run this one-line command on your CentOS 7 box:
```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/sdhuang32/c7-ovs-docker-platform/master/ovs-docker-host-setup.sh)"
```
OR, copy the script here and paste onto your CentOS 7 box and run it:
<script src="https://emgithub.com/embed.js?target=https%3A%2F%2Fgithub.com%2Fsdhuang32%2Fc7-ovs-docker-platform%2Fblob%2Fmaster%2Fovs-docker-host-setup.sh&style=github&showBorder=on&showLineNumbers=on&showFileMeta=on&showCopy=on"></script>
    * It will do the following:
        * Install openvswitch
        * Setup ovs bridge `br-ext`
        * Install docker-ce

Now you are all set and ready to use this host for future development!

## Play with your containerized applications
### Example to run:
```bash
$ docker run --name nginx --hostname nginx --net=none -d nginx # Build beforehand if you use a customized image
# Before running the following command,
# "route -n" to find out the default gateway of your network,
# and "arp-scan -l" or "arp-scan -I <eth ID> -l" to find out available IPs.
$ ovs-docker add-port br-ext eth0 nginx --ipaddress=192.168.81.102/24 --gateway=192.168.81.254
```
Then you can hit `http://192.168.81.102/` in your browser to access your nginx service, 
and don't need to rely on the port forwarding mechanism. 

This is helpful because we separate services by IPs as opposed to ports, 
and we can just access them by standard ports, in a standard manner, and don't need to customize the clients. 
It's much more aligned to general use cases.  
You can repeat above commands and apply the same logic to other containers, and enjoy the ease of interconnectivity! 

### Example to stop:
```bash
$ ovs-docker del-port br-ext eth0 nginx
$ docker stop nginx # docker start nginx later if you want
$ docker rm nginx # permanently remove this container and its non-volume/non-bind-mount data
```

### Note
As you can see, your containerized services rely on the Docker daemon running on a single Linux node, 
so this method does not provide high availability for your services. 
Also, you need to manage the virtual network interfaces (veth devices) via `ovs-docker` as mentioned above 
when creating/stopping containers (and maybe directly via `ip` and `ovs-vsctl` commands when there is an error). 
As a result, this is only suitable for local development/testing purposes and not recommended 
for large scale, long term, or mission-critical internal services.


[1]: <https://kb.vmware.com/s/article/1002934>
[2]: <https://kb.vmware.com/s/article/1004099>
