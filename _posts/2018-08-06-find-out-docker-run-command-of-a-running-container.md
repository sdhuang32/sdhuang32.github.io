---
title:  "Find out complete 'docker run' options of a running container"
excerpt: "I'm going to share how to show the full 'docker run' command including all options 
of a running container using a tool by Assaf Lavie." 
classes: wide
tags:
  - Linux
  - Docker
  - Tools
date: 2018-08-06
last_modified_at: 2019-08-13
---

---
這篇文章的繁體中文版本請看[這裡](/zh-tw/find-out-docker-run-command-of-a-running-container)。

---

Sometimes we see a running container and we wonder how it is created and started, and if we know it we can run a same container to test, or slightly modify some options to suit other circumstances.

Can we know the complete options from the running container itself instead of checking the source of your colleagues or original authors? 
Refer to [this post](https://stackoverflow.com/questions/32758793/how-to-show-the-run-command-of-a-docker-container)，there is a [project](https://github.com/lavie/runlike) mentioned to fulfill this need!

Usage:
```bash
$ docker run --rm -v /var/run/docker.sock:/var/run/docker.sock assaflavie/runlike <CONTAINER_ID>
```
The following example is a standard Centos container with systemd enabled:
```bash
$ docker run --name test --hostname test --privileged -d sdhuang32/c7-systemd
3f90ddfa39f9594deaf29fca86d49d2387e79e1dce0ff81b1fa5c9232a22343c

$ docker ps | grep test
3f90ddfa39f9        sdhuang32/c7-systemd       "/usr/sbin/init"         35 seconds ago      Up 30 seconds                                       test

$ docker run --rm -v /var/run/docker.sock:/var/run/docker.sock assaflavie/runlike 3f90ddfa39f9
docker run --name=test --hostname=test --env="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin" --env="container=docker" --volume="/sys/fs/cgroup" --privileged --restart=no --label org.label-schema.schema-version="1.0" --label org.label-schema.license="GPLv2" --label org.label-schema.vendor="CentOS" --label org.label-schema.build-date="20181205" --label org.label-schema.name="CentOS Base Image" --detach=true sdhuang32/c7-systemd /usr/sbin/init
```
