---
title:  "Retrieve Dockerfile from existing Docker image"
excerpt: "In this article I'm going to share how to generate a close-to-original Dockerfile 
of a Docker image using a useful tool by Luka Peschke."
classes: wide
tags: 
  - Docker
  - Tools
date: 2018-06-19
last_modified_at: 2019-08-13
---

---
這篇文章的繁體中文版本請看[這裡](/zh-tw/dockerfile-from-docker-image)。

---

When we are working with Docker containers whose images are provided by others,
we often wonder how the features are achieved, namely we wanna know the content in each image layer.

If we can easily get the Dockerfile from the original provider, then we know the tricks and we can even
modify the Dockerfile to fit our own needs.
But if not... of course we can use built-in "Docker history"
command to show the stuff according to each layer's metadata. 
Apart from that, there are also some projects that help us retrieve a close-to-original Dockerfile from
an existing Docker image.

```
1. https://github.com/CenturyLinkLabs/dockerfile-from-image
2. https://github.com/lukapeschke/dockerfile-from-image
```

The first one is too old and seems not compatible with newer version of Docker.
([Reference](https://github.com/CenturyLinkLabs/dockerfile-from-image/issues/14#issuecomment-272294267))
Following the reference mentioned above, I found the second solution and it seemed to work great.

```bash
$ git clone https://github.com/lukapeschke/dockerfile-from-image.git
$ cd dockerfile-from-image/
$ docker build --rm -t lukapeschke/dockerfile-from-image .

$ docker pull ruby
$ docker image | grep ruby
REPOSITORY                                    TAG                 IMAGE ID            CREATED              SIZE
docker.io/ruby                                latest              d529acb9f124        4 weeks ago          840 MB

$ docker run --rm -v '/var/run/docker.sock:/var/run/docker.sock' lukapeschke/dockerfile-from-image d529acb9f124
FROM docker.io/ruby:latest
ADD file:2cddee716e84c40540a69c48051bd2dcf6cd3bd02a3e399334e97f20a77126ff in /
CMD ["bash"]
RUN /bin/sh -c apt-get update \
    && apt-get install -y --no-install-recommends 		ca-certificates 		curl 		netbase 		wget 	\
    && rm -rf /var/lib/apt/lists/*
RUN /bin/sh -c set -ex; 	if ! command -v gpg > /dev/null; then 		apt-get update; 		apt-get install -y --no-install-recommends 			gnupg 			dirmngr 		; 		rm -rf /var/lib/apt/lists/*; 	fi
RUN /bin/sh -c apt-get update \
    && apt-get install -y --no-install-recommends 		git 		mercurial 		openssh-client 		subversion 	procps 	\
    && rm -rf /var/lib/apt/lists/*
RUN /bin/sh -c set -ex; 	apt-get update; 	apt-get install -y --no-install-recommends 		autoconf 		automake 		bzip2 		dpkg-dev 		file 		g++ 		gcc 		imagemagick 		libbz2-dev 	libc6-dev 		libcurl4-openssl-dev 		libdb-dev 		libevent-dev 		libffi-dev 		libgdbm-dev 	libgeoip-dev 		libglib2.0-dev 		libgmp-dev 		libjpeg-dev 		libkrb5-dev 		liblzma-dev 		libmagickcore-dev 		libmagickwand-dev 		libncurses5-dev 		libncursesw5-dev 		libpng-dev 	libpq-dev 		libreadline-dev 		libsqlite3-dev 		libssl-dev 		libtool 		libwebp-dev 	libxml2-dev 		libxslt-dev 		libyaml-dev 		make 		patch 		unzip 		xz-utils 		zlib1g-dev 				$( 			if apt-cache show 'default-libmysqlclient-dev' 2>/dev/null | grep -q '^Version:'; then 				echo 'default-libmysqlclient-dev'; 			else 				echo 'libmysqlclient-dev'; 			fi 		) 	; 	rm -rf /var/lib/apt/lists/*
RUN /bin/sh -c set -eux; 	mkdir -p /usr/local/etc; 	{ 		echo 'install: --no-document'; 		echo 'update: --no-document'; 	} >> /usr/local/etc/gemrc
ENV RUBY_MAJOR=2.6
ENV RUBY_VERSION=2.6.3
ENV RUBY_DOWNLOAD_SHA256=11a83f85c03d3f0fc9b8a9b6cad1b2674f26c5aaa43ba858d4b0fcc2b54171e1
RUN /bin/sh -c set -eux; 		savedAptMark="$(apt-mark showmanual)"; 	apt-get update; 	apt-get install -y --no-install-recommends 		bison 		dpkg-dev 		libgdbm-dev 		ruby 	; 	rm -rf /var/lib/apt/lists/*; 		wget -O ruby.tar.xz "https://cache.ruby-lang.org/pub/ruby/${RUBY_MAJOR%-rc}/ruby-$RUBY_VERSION.tar.xz"; 	echo "$RUBY_DOWNLOAD_SHA256 *ruby.tar.xz" | sha256sum --check --strict; 		mkdir -p /usr/src/ruby; 	tar -xJf ruby.tar.xz -C /usr/src/ruby --strip-components=1; 	rm ruby.tar.xz; 		cd /usr/src/ruby; 		{ 		echo '#define ENABLE_PATH_CHECK 0'; 		echo; 		cat file.c; 	} > file.c.new; 	mv file.c.new file.c; 		autoconf; 	gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)"; 	./configure 		--build="$gnuArch" 		--disable-install-doc 		--enable-shared 	; 	make -j "$(nproc)"; 	make install; 		apt-mark auto '.*' > /dev/null; 	apt-mark manual $savedAptMark > /dev/null; 	find /usr/local -type f -executable -not \( -name '*tkinter*' \) -exec ldd '{}' ';' 		| awk '/=>/ { print $(NF-1) }' 		| sort -u 		| xargs -r dpkg-query --search 		| cut -d: -f1 		| sort -u 		| xargs -r apt-mark manual 	; 	apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; 		cd /; 	rm -r /usr/src/ruby; 	! dpkg -l | grep -i ruby; 	[ "$(command -v ruby)" = '/usr/local/bin/ruby' ]; 	ruby --version; 	gem --version; 	bundle --version
ENV GEM_HOME=/usr/local/bundle
ENV BUNDLE_PATH=/usr/local/bundle BUNDLE_SILENCE_ROOT_WARNING=1 BUNDLE_APP_CONFIG=/usr/local/bundle
ENV PATH=/usr/local/bundle/bin:/usr/local/bundle/gems/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
RUN /bin/sh -c mkdir -p "$GEM_HOME" \
    && chmod 777 "$GEM_HOME"
CMD ["irb"]
```

Note that this tool cannot retrieve the files added into the image through COPY or ADD instructions.
The following example is using this tool to retrieve *its own* Dockerfile:
```bash
$ docker images | grep "lukapeschke/dockerfile-from-image"
lukapeschke/dockerfile-from-image             latest              d719f8dcb798        37 minutes ago      59 MB

$ docker run -v /var/run/docker.sock:/var/run/docker.sock lukapeschke/dockerfile-from-image d719f8dcb798
FROM docker.io/alpine:latest
RUN /bin/sh -c apk add --update python3 wget      \
    && wget -O - --no-check-certificate https://bootstrap.pypa.io/get-pip.py | python3      \
    && apk del wget      \
    && pip3 install -U docker-py      \
    && yes | pip3 uninstall pip
COPY file:d7369c0379dc34ec79c308a782b14eab9c86ed1ebc41b5ce859e32760518fb21 in /root
ENTRYPOINT ["/root/entrypoint.py"]
```
All we know is that there is one file got copyed into the image but we don't know what it is anyway.
