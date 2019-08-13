---
title:  "由執行中的 container 找出執行它的 docker run 指令下法"
classes: wide
tags: 
  - Docker
  - Tools
date: 2018-08-06
---

---
 For English version of this post please see [HERE](/find-out-docker-run-command-of-a-running-container).

---

如果在機器上看到一個已經跑起來的 container，我們想知道他 docker run 的參數是怎麼下的，這樣就可以開一個一模一樣的 container 出來做實驗，或者是小改一點參數來符合自己的需求，這種時候除了問同事或找作者的 source code 看他怎麼給參數以外，可不可以直接從已經在跑的 container 看出端倪呢?

參考[這篇](https://stackoverflow.com/questions/32758793/how-to-show-the-run-command-of-a-docker-container)，底下回答提到已經有[專案](https://github.com/lavie/runlike)在做這件事!

他已經做成一個 docker images `assaflavie/runlik` 了，可以直接用它來執行，用法:
```bash
$ docker run --rm -v /var/run/docker.sock:/var/run/docker.sock assaflavie/runlike <CONTAINER_ID>
```
以下用一個官方範例的 Centos container 來測試：
```bash
$ docker run --name test --hostname test --privileged -d sdhuang32/c7-systemd
3f90ddfa39f9594deaf29fca86d49d2387e79e1dce0ff81b1fa5c9232a22343c

$ docker ps | grep test
3f90ddfa39f9        sdhuang32/c7-systemd       "/usr/sbin/init"         35 seconds ago      Up 30 seconds                                       test

$ docker run --rm -v /var/run/docker.sock:/var/run/docker.sock assaflavie/runlike 3f90ddfa39f9
docker run --name=test --hostname=test --env="PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin" --env="container=docker" --volume="/sys/fs/cgroup" --privileged --restart=no --label org.label-schema.schema-version="1.0" --label org.label-schema.license="GPLv2" --label org.label-schema.vendor="CentOS" --label org.label-schema.build-date="20181205" --label org.label-schema.name="CentOS Base Image" --detach=true sdhuang32/c7-systemd /usr/sbin/init
```
