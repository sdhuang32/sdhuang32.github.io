---
title:  "Install Git on QNAP QTS"
excerpt: "I'm going to share a useful tip to install official Git utility directly on QNAP QTS, 
which can definitely make your NAS more developer-friendly."
classes: wide
tags:
  - Linux
  - QNAP 
  - QTS
  - Container Station
  - Docker
  - Containers
  - Tools
date: 2018-09-25
last_modified_at: 2019-08-24
---

If you work on a QNAP QTS NAS "directly", I mean, just ssh into it and use it as your 
official Linux box, you may find it a bit hard because QTS is a specialized embedded Linux. 
There is only a limited number of utilities built-in and, although it is based on Ubuntu, 
it has neither package manager such as apt or yum nor compiling tools such as gcc and make. 
So it's not easy to install a new package unless there is a QPKG provided to install the one you want.

Git is one of those packages that you desire to install so that you can develop your stuff 
more comfortably on your NAS. But there is no official Git QPKG on the AppCenter... so let's 
build it on our own!

The concept is simple.
```
1. Start a container with a bind mount from QTS
2. Build Git from source inside the container with prefix set as the bind mount directory
3. Install into the bind mount directory
4. Set the PATH for BASH on QTS
```

Let me give you an implementation that works on my machine. 
I use Container Station which can be installed through AppCenter to start containers, 
so if you are trying this solution you have to install it first. 
`system-docker` is the CLI utility of Container Station that works as the same as `docker`. 
Finally the artifacts will be installed into `/share/Public/toolchain/` directory.
My QTS firmware version: `4.3.6.0979 build 20190620`, Container Station version: `V1.9.3527`, target Git version: `2.23.0`

The following are my scripts. The first one (`install-git-on-qts.sh`) is main script and 
the latter one (`build-git.sh`) is to be copyed into the container to build and install Git.

<script src="https://gist.github.com/sdhuang32/b64cda152c005487c151ee55e458ab2c.js"></script>

<script src="https://gist.github.com/sdhuang32/349d55ace0324e57681df01c1742038f.js"></script>

You can download above two scripts on your NAS, place them in the same directory, 
and execute `path/to/install-git-on-qts.sh`. If it finishes without any error, 
you should be able to execute `git` command in a new ssh session!
