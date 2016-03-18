---
layout: post
title:  "More about self-made Docker images: Alpine Linux"
excerpt: "Blog Quickie: Short example building a Alpine Linux Docker image"
date:   2016-03-18 22:18:00
categories: blog
share: true
comments: true
tags:
- docker
- image
- alpinelinux
- java8
- microservices
- howto
---

In my blog article from yesterday about [Docker images from scratch](https://www.sam6.de/articles/Docker-from-scratch/) I used Debian. The result was a quite small [image for Docker](https://hub.docker.com/r/bundeskanzler4711/jessie-minbase/).

But in my opinion it is too big. So there must be a "better" way to a smaller image.

First I had a look at Busybox, a real small Linux - but without a package manager. Another Distro called Alpine Linux faces this issue. It is Busybox based and adds a simple package manager called `apk`.

In short, I executed these steps in my local workspace folder:

    mkdir alpinlinux
    cd alpinlinux/
    mkdir root
    curl -sSL http://nl.alpinelinux.org/alpine/latest-stable/main/x86_64/apk-tools-static-$(curl -sSL http://nl.alpinelinux.org/alpine/latest-stable/main/x86_64/APKINDEX.tar.gz | tar -Oxz | grep --text '^P:apk-tools-static$' -A1 | tail -n1 | cut -d: -f2).apk | tar -xz -C . sbin/apk.static
    ./sbin/apk.static --repository http://nl.alpinelinux.org/alpine/latest-stable/main/ --update-cache --allow-untrusted --root ./root --initdb add alpine-base
    echo "http://nl.alpinelinux.org/alpine/latest-stable/main/" > ./root/etc/apk/repositories
    echo "http://nl.alpinelinux.org/alpine/latest-stable/community/" >> ./root/etc/apk/repositories
    tar --numeric-owner -C root -c . | docker import - alpine:0
    
At this point the Alpine Linux base image for docker is finished. As yesterday I pushed my image to Docker Hub so that you can use it easily: [LINK](https://hub.docker.com/r/bundeskanzler4711/alpine-base/)

As second step I built a very small OpenJDK 8 JRE image based on this. Therefore I used a Dockerfile like:

    FROM bundeskanzler4711/alpine-base:latest
    RUN apk add --no-cache openjdk8-jre

Yes, that's it. In a few seconds my new image was ready:

    $ docker build -t bundeskanzler4711/alpine-base:0-java8 .
    Sending build context to Docker daemon 2.048 kB
    Step 1 : FROM bundeskanzler4711/alpine-base:latest
     ---> 3337a006116b
    Step 2 : RUN apk add --no-cache openjdk8-jre
     ---> Running in 30bd48d54d95
    fetch http://nl.alpinelinux.org/alpine/latest-stable/main/x86_64/APKINDEX.tar.gz
    fetch http://nl.alpinelinux.org/alpine/latest-stable/community/x86_64/APKINDEX.tar.gz
    (1/24) Installing libffi (3.2.1-r2)
    (2/24) Installing libtasn1 (4.7-r0)
    (3/24) Installing p11-kit (0.23.1-r1)
    (4/24) Installing p11-kit-trust (0.23.1-r1)
    (5/24) Installing openssl (1.0.2g-r0)
    (6/24) Installing ca-certificates (20160104-r2)
    (7/24) Installing java-cacerts (1.0-r0)
    (8/24) Installing libxau (1.0.8-r1)
    (9/24) Installing libxdmcp (1.1.2-r1)
    (10/24) Installing libxcb (1.11.1-r0)
    (11/24) Installing libx11 (1.6.3-r2)
    (12/24) Installing libxext (1.3.3-r1)
    (13/24) Installing libxi (1.7.5-r0)
    (14/24) Installing libxrender (0.9.9-r1)
    (15/24) Installing libxtst (1.2.2-r0)
    (16/24) Installing libpng (1.6.20-r0)
    (17/24) Installing freetype (2.6.2-r0)
    (18/24) Installing libgcc (5.3.0-r0)
    (19/24) Installing giflib (5.1.1-r0)
    (20/24) Installing openjdk8-jre-lib (8.72.15-r2)
    (21/24) Installing java-common (0.1-r0)
    (22/24) Installing alsa-lib (1.1.0-r1)
    (23/24) Installing openjdk8-jre-base (8.72.15-r2)
    (24/24) Installing openjdk8-jre (8.72.15-r2)
    Executing busybox-1.24.1-r7.trigger
    Executing ca-certificates-20160104-r2.trigger
    Executing java-common-0.1-r0.trigger
    OK: 106 MiB in 40 packages
     ---> 3b0ce8bcd143
    Removing intermediate container 30bd48d54d95
    Successfully built 3b0ce8bcd143

I am very proud. ;-)
Finally, it REALLY works:

    $ docker run bundeskanzler4711/alpine-base:latest-java8 java -version
    openjdk version "1.8.0_72-internal"
    OpenJDK Runtime Environment (build 1.8.0_72-internal-alpine-r2-b15)
    OpenJDK 64-Bit Server VM (build 25.72-b15, mixed mode)

As described above, you can find all images at [Docker Hub](https://hub.docker.com/u/bundeskanzler4711/).
In the next time I will write a short tutorial on how to build a Java Microservice with Docker using Maven.