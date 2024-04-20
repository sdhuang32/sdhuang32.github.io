---
title:  "oVirt HA mechanism"
excerpt: "I'm going to share my study about three HA features in oVirt: 
Engine HA, Guest HA, and Host HA." 
classes: wide
tags:
  - oVirt
  - High Availability
date: 2017-11-08
last_modified_at: 2019-08-26
---

[oVirt](https://en.wikipedia.org/wiki/OVirt) enables you to establish a hyper-converged infrastructure (HCI), 
namely to setup and manage virtual machines, virtual network, 
and storage resources on top of a distributed KVM-based Linux system. 
In order to provide high availability, There are three features for different entities respectively:

**1. Engine HA**: oVirt engine is the management part of oVirt, which records and manages all resources. 
oVirt natively supports the "[self-hosted engine](https://ovirt.org/documentation/self-hosted/Self-Hosted_Engine_Guide.html)", 
which is to install oVirt engine inside a VM so oVirt engine can manage that VM where itself is running (also called Engine VM). 
If the Engine VM goes down, hosts (hypervisor nodes) within the same oVirt cluster will cooperate 
to select a proper host to rerun the Engine VM.

**2. Guest HA**: As long as a guest VM is marked by the administrator as "highly available", 
when that VM is down unexpectedly (not normally shut down by an end user or administrator), 
oVirt engine will keep trying to start it again on either the previous host or another host.

**3. Host HA**: If a host (hypervisor node) becomes non-responsive due to power failure, network outage, etc, 
oVirt engine will fence that host by sending an [IPMI](https://en.wikipedia.org/wiki/Intelligent_Platform_Management_Interface) 
stop request through a dedicated network channel to the power management unit of the host, 
and then start it again after it becomes down successfully.

The following is my study about three cases above, 
which adopts self-hosted engine and clustered storage (such as Ceph or GlusterFS).

<div class="embed-container"
 style="position: relative; padding-bottom: 59.27%; height: 0; overflow: hidden; max-width: 100%;">
 <iframe style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"
  src="https://docs.google.com/presentation/d/e/2PACX-1vQPx8RwCrjYeeLCubXmJT5dqXtw974-MvGrj8uFg-vwTE0YjZs1Pmbkuz6oIcvW4rXbXGQEtsqLBDaq/embed?start=false&loop=false&delayms=3000" frameborder="0" width="960" height="569" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>
</div>
