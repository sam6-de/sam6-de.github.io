---
layout: post
title: Docker images from scratch
excerpt: "This is an example for beginners, showing how to build your own Docker image using `debootstrap` and `tar`"
modified: 2016-03-17
categories: articles
tags:
- docker
- image
- debian
- java8
- microservices
- icinga
- debootstrap
comments: true
share: true
---

# Fork me on GitHub
My `Dockerfile` examples are also available on [GitHub](https://github.com/sam6-de/docker-from-scratch) 

# Before we start
If you are only interested in running or modifying my example Dockerfiles, you can do so. But keep in mind that I will not take care of offering bug- or security-issue-free images in any of my freely available repositories. I only want to give you some basic ideas of how Docker and its images work. 

# Prerequisites
My test setup consists of:

- [`Docker 1.6.2`](https://www.docker.com/open-source)
- [`Debian 8.3`](https://www.debian.org)
- [`debootstrap 1.0.67`](https://wiki.debian.org/Debootstrap)

# Quickstart
To prepare and run your custom Docker image, there are only three steps to do

1. Install Debian Jessie into a local folder `debootstrap --variant=minbase jessie ./jessie-minbase http://ftp.debian.org/debian/`
2. Import the folder as a new image into Docker `tar -C jessie-minbase -c . | docker import - jessie-minbase`
3. Run your own Debian Docker image `docker run jessie-minbase cat /etc/debian_version`

That's it!

# Technical background

## Debian Jessie in a folder
As you can see it is very easy to install any Debian based system into a folder. You could even do a `chroot` into your directory and install some packages, modify config files or whatever you want to do. These changes would be part of the Docker image, if you would import it afterwards.
But keep cool: we will do some better stuff in a few minutes ;-)

First let's understand, what my parameters will do:

    debootstrap --variant=minbase jessie ./jessie-minbase http://ftp.debian.org/debian/

- `debootstrap` is the call of the Debian bootstrapping tool
- `--variant=minbase`: Name of the bootstrap script variant to use. Currently, the variants supported are minbase, which only includes essential packages and apt; buildd, which installs the build-essential packages into your target directory and fakechroot, which installs the packages without root privileges. Finally there is variant scratchbox, which is for creating targets for scratchbox usage. The default, with no --variant=X argument, is to create a base Debian installation in your target folder.
- `jessie` is the name of the suite. The suite may be a release code name (eg, sid, wheezy, squeeze, lenny) or a symbolic name (eg, unstable, testing, stable, oldstable).
- `./jessie-minbase` points to your target folder, where the Debian system will be installed to.
- `http://ftp.debian.org/debian/` gives the mirror to be used during installation process and in your machine. The mirror can be an `http://` url, a `file:///` url, or an `ssh:///` url and must be reachable by `debootstrap`. Feel free to choose another mirror, nearby your location (for example `http://ftp.de.debian.org/debian` in Germany).

### Example output
Here is a shortened example output:

    me@mydebianbox:~/docker# debootstrap --variant=minbase jessie ./jessie-minbase http://ftp.debian.org/debian/
    I: Retrieving Release 
    I: Retrieving Release.gpg 
    I: Checking Release signature
    I: Valid Release signature (key id 75DDC3C4A499F1A18CB5F3C8CBF8D6FD518E17E1)
    I: Retrieving Packages 
    I: Validating Packages 
    I: Resolving dependencies of required packages...
    I: Resolving dependencies of base packages...
    I: Found additional required dependencies: acl adduser dmsetup insserv libaudit-common libaudit1 libbz2-1.0 libcap2 libcap2-bin libcryptsetup4 libdb5.3 libdebconfclient0 libdevmapper1.02.1 libgcrypt20 libgpg-error0 libkmod2 libncursesw5 libprocps3 libsemanage-common libsemanage1 libslang2 libsystemd0 libudev1 libustr-1.0-1 procps systemd systemd-sysv udev 
    I: Found additional base dependencies: debian-archive-keyring gnupg gpgv libapt-pkg4.12 libreadline6 libstdc++6 libusb-0.1-4 readline-common 
    I: Checking component main on http://ftp.debian.org/debian...
    I: Retrieving acl 2.2.52-2
    I: Validating acl 2.2.52-2
    [...]
    I: Retrieving zlib1g 1:1.2.8.dfsg-2+b1
    I: Validating zlib1g 1:1.2.8.dfsg-2+b1
    I: Chosen extractor for .deb packages: dpkg-deb
    I: Extracting acl...
    [...]
    I: Extracting zlib1g...
    I: Installing core packages...
    I: Unpacking required packages...
    I: Unpacking acl...
    [...]
    I: Unpacking zlib1g:amd64...
    I: Configuring required packages...
    I: Configuring gcc-4.8-base:amd64...
    [...]
    I: Configuring libc-bin...
    I: Unpacking the base system...
    I: Unpacking apt...
    [...]
    I: Unpacking readline-common...
    I: Configuring the base system...
    I: Configuring readline-common...
    [...]
    I: Configuring libc-bin...
    I: Base system installed successfully.
    me@mydebianbox:~/docker#

## TAR as image
The tool `tar` is used to create a temporary tarball of your local Debian installation folder. This tarball can easily be imported as Docker image:

    tar -C jessie-minbase -c . | docker import - jessie-minbase

- `tar`: This tool saves many files together into a single tape or disk archive, and can restore individual files from the archive.
- `-C jessie-minbase`: Change into directory `jessie-minbase` and use this as source folder
- `-c .`: Create a new archive, and use this (`.`, i.e. the current folder `jessie-minbase`) as source.
- `|` pipes the output to the next command
- `docker import` starts the import tool from Docker
- `-` instructs Docker to read from the command line
- `jessie-minbase` is the name of the recently created Docker image. It is not necessary to choose the same name as the folder has, but in my opinion it is easier to see where the image comes from. But feel free to choose a different name at this point.

### Example output
Unlike the first step, the output of the import is really short and is just one line:

    me@mydebianbox:~/docker# tar -C jessie-minbase -c . | docker import - jessie-minbase
    ea3ac0df773d3cba1dc4901938d436b37acf0663dc7287a3ed1dd3a87b9ff8f2
    me@mydebianbox:~/docker#

## Run your own container
Now the hard stuff is done. Debian was successfully installed into a folder, we created a temporary archive of this folder and imported it into Docker as new image.
To check if everything went well, this step is only a quick check if our image can run:

    docker run jessie-minbase cat /etc/debian_version
    
- `docker run` runs a command in a new container.
- `jessie-minbase` is the name of the image we created in the steps above.
- `cat /etc/debian_version` is the command to be executed within the container. In this example it prints the Debian version of the container to STDOUT.

### Example output
As expected we created a Debian Jessie image in version 8.3:

    me@mydebianbox:~/docker# docker run jessie-minbase cat /etc/debian_version
    8.3
    me@mydebianbox:~/docker# 
    
The current version may differ from yours if Debian releases a newer version of Jessie.

# Publish your image

## Login
To be able to `push` your image to a registry like [DockerHub](https://docs.docker.com/mac/step_five/) first you have to log in. This is simply done by 

    docker login

Note: Your authentication credentials will be stored in the `.dockercfg` authentication file in your home directory.

## Tag your image
Type `docker images` to list the images you currently have:

    me@mydebianbox:~/docker# docker images
    REPOSITORY                                 TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
    jessie-minbase                             latest              bca6396f92bf        2 hours ago         157.6 MB
    
Find the IMAGE ID for your `jessie-minbase` image. In this example, the id is `bca6396f92bf`. Another information you need is your username for Docker Hub (mine is `bundeskanzler4711`) and the Version. In this example I will tag your image twice: one time with the Debian version number (e.g. 8.3) and second as `latest` version. The second one can be moved to a newer version in the future.

    docker tag bca6396f92bf bundeskanzler4711/jessie-minbase:8.3
    docker tag bca6396f92bf bundeskanzler4711/jessie-minbase:latest
    
As result the same IMAGE ID will occur three times in the images listing:

    me@mydebianbox:~/docker# docker images
    REPOSITORY                                 TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
    bundeskanzler4711/jessie-minbase           8.3                 bca6396f92bf        2 hours ago         157.6 MB
    bundeskanzler4711/jessie-minbase           latest              bca6396f92bf        2 hours ago         157.6 MB
    jessie-minbase                             latest              bca6396f92bf        2 hours ago         157.6 MB
    
## Push your image
At this point you should be logged in at any Docker registry. In my case I will upload my images to a public DockerHub repository, i.e. can be pulled freely without login.

    me@mydebianbox:~/docker# docker push bundeskanzler4711/jessie-minbase
    The push refers to a repository [bundeskanzler4711/jessie-minbase] (len: 2)
    bca6396f92bf: Image already exists 
    bca6396f92bf: Image already exists 
    Digest: sha256:6f8757ad19ad16fa5140520e098aeccd15f2721dc6965e339e2ee33accd97513
    me@mydebianbox:~/docker#

The pushed image is really small, in my case around 59 MB. You can see all tags that are related with my image [online](https://hub.docker.com/r/bundeskanzler4711/jessie-minbase/tags/).

# Use image as new base

## Dockerfile

### Step 1
If you want to keep your image up to date, you can use a Dockerfile. In this case it could be like this:

    FROM bundeskanzler4711/jessie-minbase:latest
    
    RUN apt-get -y update
    RUN apt-get -y --force-yes dist-upgrade
    RUN apt-get -y --force-yes autoremove
    RUN apt-get clean
    
### Step 2
To execute the Dockerfile, switch into the directory where your file is located and run

    me@mydebianbox:~/docker# docker build -t jessie-minbase-test-1 .
    Sending build context to Docker daemon 2.048 kB
    Step 1 : FROM bundeskanzler4711/jessie-minbase:latest
     ---> fe6449c63150
    Step 2 : RUN apt-get -y update
     ---> Running in 6fe6c3322ea7
    Ign http://ftp.de.debian.org jessie InRelease
    Hit http://ftp.de.debian.org jessie Release.gpg
    Hit http://ftp.de.debian.org jessie Release
    Get:1 http://ftp.de.debian.org jessie/main amd64 Packages [6763 kB]
    Get:2 http://ftp.de.debian.org jessie/main Translation-en [4582 kB]
    Fetched 11.3 MB in 14s (790 kB/s)
    Reading package lists...
     ---> 3d19b912a922
    Removing intermediate container 6fe6c3322ea7
    Step 3 : RUN apt-get -y --force-yes dist-upgrade
     ---> Running in 5aa0a49c1f56
    Reading package lists...
    Building dependency tree...
    0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
     ---> 6674a4c2e6e2
    Removing intermediate container 5aa0a49c1f56
    Step 4 : RUN apt-get -y --force-yes autoremove
     ---> Running in a083000e1b6b
    Reading package lists...
    Building dependency tree...
    0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
     ---> b368380f8490
    Removing intermediate container a083000e1b6b
    Step 5 : RUN apt-get clean
     ---> Running in 1698a5360db7
     ---> 3a80b3f0cb76
    Removing intermediate container 1698a5360db7
    Successfully built 3a80b3f0cb76
    me@mydebianbox:~/docker#
    
### Step 3
So far, so good. But what happens, if it will be executed another time?

    me@mydebianbox:~/docker# docker build -t jessie-minbase-test-2 .
    Sending build context to Docker daemon 2.048 kB
    Step 1 : FROM bundeskanzler4711/jessie-minbase:latest
     ---> fe6449c63150
    Step 2 : RUN apt-get -y update
     ---> Using cache
     ---> 3d19b912a922
    Step 3 : RUN apt-get -y --force-yes dist-upgrade
     ---> Using cache
     ---> 6674a4c2e6e2
    Step 4 : RUN apt-get -y --force-yes autoremove
     ---> Using cache
     ---> b368380f8490
    Step 5 : RUN apt-get clean
     ---> Using cache
     ---> 3a80b3f0cb76
    Successfully built 3a80b3f0cb76
    me@mydebianbox:~/docker#
    
Wow, really fast! But ... the IMAGE IDs are exactly the same as in the first run. In other words: nothing was executed, because Docker thinks, that these steps were already executed before. Docker is right, of course. But this is not the behavior we expected. To enforce re-execution of the steps in the Dockerfile, the parameter `--no-cache=true` has to be passed to the Docker command.

    me@mydebianbox:~/docker# docker build --no-cache=true -t jessie-minbase-test-3 .
    Sending build context to Docker daemon 2.048 kB
    Step 1 : FROM bundeskanzler4711/jessie-minbase:latest
     ---> fe6449c63150
    Step 2 : RUN apt-get -y update
     ---> Running in bc9eaa66570a
    Ign http://ftp.de.debian.org jessie InRelease
    Hit http://ftp.de.debian.org jessie Release.gpg
    Hit http://ftp.de.debian.org jessie Release
    Get:1 http://ftp.de.debian.org jessie/main amd64 Packages [6763 kB]
    Get:2 http://ftp.de.debian.org jessie/main Translation-en [4582 kB]
    Fetched 11.3 MB in 13s (869 kB/s)
    Reading package lists...
     ---> 1e538f7d94cd
    Removing intermediate container bc9eaa66570a
    Step 3 : RUN apt-get -y --force-yes dist-upgrade
     ---> Running in 8c5dbcb27c12
    Reading package lists...
    Building dependency tree...
    0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
     ---> e97943aa5c2f
    Removing intermediate container 8c5dbcb27c12
    Step 4 : RUN apt-get -y --force-yes autoremove
     ---> Running in 4a4884672e20
    Reading package lists...
    Building dependency tree...
    0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
     ---> fedbe2fd18da
    Removing intermediate container 4a4884672e20
    Step 5 : RUN apt-get clean
     ---> Running in 75d1aa74a5b0
     ---> 08c991fb2ee8
    Removing intermediate container 75d1aa74a5b0
    Successfully built 08c991fb2ee8
    me@mydebianbox:~/docker# 
    
### Step 4    
Yeah, that's how it should be. Finally: tagging and pushing.

    me@mydebianbox:~/docker# docker images
    REPOSITORY                             TAG                 IMAGE ID            CREATED              VIRTUAL SIZE
    jessie-minbase-test-3                  latest              08c991fb2ee8        About a minute ago   260.6 MB
    jessie-minbase-test-2                  latest              8a8479e7af79        2 minutes ago        260.6 MB
    jessie-minbase-test-1                  latest              3a80b3f0cb76        10 minutes ago       260.6 MB
    bundeskanzler4711/jessie-minbase       8.3                 bca6396f92bf        2 hours ago          157.6 MB
    bundeskanzler4711/jessie-minbase       latest              bca6396f92bf        2 hours ago          157.6 MB
    jessie-minbase                         latest              bca6396f92bf        2 hours ago          157.6 MB
    
    me@mydebianbox:~/docker# docker tag 08c991fb2ee8 bundeskanzler4711/jessie-minbase:latest
    Error response from daemon: Conflict: Tag bundeskanzler4711/jessie-minbase:latest is already set to image fe6449c63150, if you want to replace it, please use -f option
    me@mydebianbox:~/docker# docker tag -f 08c991fb2ee8 bundeskanzler4711/jessie-minbase:latest
    
    me@mydebianbox:~/docker# docker images
    REPOSITORY                             TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
    bundeskanzler4711/jessie-minbase       latest              08c991fb2ee8        3 minutes ago       260.6 MB
    jessie-minbase-test-3                  latest              08c991fb2ee8        3 minutes ago       260.6 MB
    jessie-minbase-test-2                  latest              8a8479e7af79        3 minutes ago       260.6 MB
    jessie-minbase-test-1                  latest              3a80b3f0cb76        11 minutes ago      260.6 MB
    bundeskanzler4711/jessie-minbase       8.3                 bca6396f92bf        2 hours ago         157.6 MB
    jessie-minbase                         latest              bca6396f92bf        2 hours ago         157.6 MB
    
    me@mydebianbox:~/docker# docker push bundeskanzler4711/jessie-minbase
    The push refers to a repository [docker.io/bundeskanzler4711/jessie-minbase] (len: 1)
    08c991fb2ee8: Pushed 
    fedbe2fd18da: Pushed 
    e97943aa5c2f: Pushed 
    1e538f7d94cd: Pushed 
    fe6449c63150: Image already exists 
    latest: digest: sha256:b2fb47f468659f2b99d15b4b70b30454b0470b2df7fdb3e465a614bd19d1db95 size: 7488
    me@mydebianbox:~/docker#

## Preinstall OpenJDK 8

### Step 1
My Dockerfile looks like this one:

    FROM bundeskanzler4711/jessie-minbase
    
    RUN apt-get -y update 
    RUN apt-get -y --force-yes dist-upgrade
    RUN echo "deb http://http.debian.net/debian jessie-backports main" | tee /etc/apt/sources.list.d/jessie-backports.list
    RUN apt-get update
    RUN apt-get install -y --force-yes openjdk-8-jre-headless openjdk-8-jdk
    RUN apt-get -y --force-yes autoremove
    RUN apt-get clean
    RUN /usr/sbin/update-java-alternatives -s java-1.8.0-openjdk-amd64
    
    CMD java -version


### Step 2
Equivalent to the stepse

    me@mydebianbox:~/docker# docker build -t jessie-java8 .
    Sending build context to Docker daemon 3.584 kB
    Step 1 : FROM bundeskanzler4711/jessie-minbase
     ---> 08c991fb2ee8
    Step 2 : RUN apt-get -y update
     ---> Running in 17896c221a40
    Ign http://ftp.de.debian.org jessie InRelease
    Hit http://ftp.de.debian.org jessie Release.gpg
    Hit http://ftp.de.debian.org jessie Release
    Hit http://ftp.de.debian.org jessie/main amd64 Packages
    Hit http://ftp.de.debian.org jessie/main Translation-en
    Reading package lists...
     ---> ba5f38f8465e
    Removing intermediate container 17896c221a40
    Step 3 : RUN apt-get -y --force-yes dist-upgrade
     ---> Running in 711e944349e2
    Reading package lists...
    Building dependency tree...
    0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
     ---> ccebe30c9716
    Removing intermediate container 711e944349e2
    Step 4 : RUN echo "deb http://http.debian.net/debian jessie-backports main" | tee /etc/apt/sources.list.d/jessie-backports.list
     ---> Running in 83c1a7f01d2d
    deb http://http.debian.net/debian jessie-backports main
     ---> 786a28d7b738
    Removing intermediate container 83c1a7f01d2d
    Step 5 : RUN apt-get update
     ---> Running in 95cbf4d4d759
    Ign http://ftp.de.debian.org jessie InRelease
    Hit http://ftp.de.debian.org jessie Release.gpg
    Hit http://ftp.de.debian.org jessie Release
    Get:1 http://http.debian.net jessie-backports InRelease [166 kB]
    Hit http://ftp.de.debian.org jessie/main amd64 Packages
    Hit http://ftp.de.debian.org jessie/main Translation-en
    Get:2 http://http.debian.net jessie-backports/main Translation-en [315 kB]
    Err http://http.debian.net jessie-backports/main amd64 Packages
      
    Get:3 http://http.debian.net jessie-backports/main amd64 Packages [465 kB]
    Fetched 946 kB in 3s (249 kB/s)
    Reading package lists...
     ---> 90c778b99da2
    Removing intermediate container 95cbf4d4d759
    Step 6 : RUN apt-get install -y --force-yes openjdk-8-jre-headless openjdk-8-jdk
     ---> Running in fcd6129c619d
    Reading package lists...
    Building dependency tree...
    The following extra packages will be installed:
      ca-certificates ca-certificates-java dbus dbus-x11 file fontconfig
      fontconfig-config fonts-dejavu-core fonts-dejavu-extra gconf-service gconf2
      
    [...]
    
    Processing triggers for sgml-base (1.26+nmu4) ...
     ---> 2dcd089070fa
    Removing intermediate container fcd6129c619d
    Step 7 : RUN apt-get -y --force-yes autoremove
     ---> Running in 81fc4a075333
    Reading package lists...
    Building dependency tree...
    Reading state information...
    0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
     ---> 64c4aab87f0f
    Removing intermediate container 81fc4a075333
    Step 8 : RUN apt-get clean
     ---> Running in 0b59a709f3b6
     ---> 483f0cefbb16
    Removing intermediate container 0b59a709f3b6
    Step 9 : RUN /usr/sbin/update-java-alternatives -s java-1.8.0-openjdk-amd64
     ---> Running in d8111cd10ce3
    update-alternatives: error: no alternatives for mozilla-javaplugin.so
    update-java-alternatives: plugin alternative does not exist: /usr/lib/jvm/java-8-openjdk-amd64/jre/lib/amd64/IcedTeaPlugin.so
     ---> d58f64ba252d
    Removing intermediate container d8111cd10ce3
    Step 10 : CMD java -version
     ---> Running in 1ce4850f384e
     ---> c99be2b4d042
    Removing intermediate container 1ce4850f384e
    Successfully built c99be2b4d042

### Step 3
Test the new image locally:

    me@mydebianbox:~/docker# docker run jessie-java8 java -version
    openjdk version "1.8.0_72-internal"
    OpenJDK Runtime Environment (build 1.8.0_72-internal-b15)
    OpenJDK 64-Bit Server VM (build 25.72-b15, mixed mode)

Success ;-)

### Step 3
Tag and push Java8 image

    me@mydebianbox:~/docker# docker tag -f c99be2b4d042 bundeskanzler4711/jessie-minbase:java8-latest
    me@mydebianbox:~/docker# docker tag -f c99be2b4d042 bundeskanzler4711/jessie-minbase:java8-1.8.0_72-internal
    
    docker push bundeskanzler4711/jessie-minbase
   
# A more complicated example
As last case here is a more complicated Dockerfile, installing Icinga2, Grafana and Apache2:

    FROM bundeskanzler4711/debian-jessie-base
    
    RUN apt-get -y update 
    RUN apt-get -y --force-yes dist-upgrade
    RUN echo "deb http://debmon.org/debmon debmon-jessie main" | tee /etc/apt/sources.list.d/debmon.list
    RUN wget -O - http://debmon.org/debmon/repo.key 2>/dev/null | apt-key add -
    RUN apt-get update
    RUN DEBIAN_FRONTEND=noninteractive apt-get -y --force-yes install icinga2 graphite-web graphite-carbon libapache2-mod-wsgi apache2
    
    RUN icinga2 feature enable graphite
    RUN systemctl enable icinga2.service
    RUN service icinga2 restart
    
    RUN graphite-manage syncdb --noinput
    RUN chown _graphite:_graphite /var/lib/graphite/graphite.db
    RUN a2enmod wsgi
    
    RUN echo "Listen 8000" >> /etc/apache2/ports.conf
    RUN cp /usr/share/graphite-web/apache2-graphite.conf /etc/apache2/sites-available/graphite.conf
    RUN sed -i 's/:80/:8000/g' /etc/apache2/sites-available/graphite.conf
    RUN a2ensite graphite
    RUN systemctl enable apache2.service
    RUN service apache2 restart
    
    RUN apt-get -y --force-yes install apt-transport-https
    RUN echo "deb https://packagecloud.io/grafana/stable/debian/ jessie main" | tee /etc/apt/sources.list.d/grafana.list
    RUN wget -O - https://packagecloud.io/gpg.key 2>/dev/null | apt-key add -
    RUN apt-get update
    RUN apt-get -y --force-yes install grafana
    RUN systemctl enable grafana-server.service
    RUN service grafana-server start
    
    RUN DEBIAN_FRONTEND=noninteractive apt-get -y --force-yes install mysql-server mysql-client icinga2-ido-mysql
    RUN icinga2 feature enable ido-mysql
    RUN icinga2 feature enable command
    RUN icinga2 feature list
    RUN addgroup --system icingacmd
    RUN usermod -a -G icingacmd www-data
    RUN apt-get -y --force-yes install icingaweb2
    RUN icingacli setup config webserver apache --document-root /usr/share/icingaweb2/public > /etc/apache2/sites-available/icingaweb2.conf
    RUN a2dissite 000-default.conf
    RUN a2ensite icingaweb2.conf
    RUN icingacli setup config directory --group icingaweb2;
    RUN icingacli setup token create;
    RUN service apache2 restart
    
    RUN apt-get -y --force-yes autoremove
    RUN apt-get clean
    
    EXPOSE 80
    EXPOSE 8000
    EXPOSE 3000
    
    ENTRYPOINT service icinga2 start
    ENTRYPOINT service grafana-server start
    ENTRYPOINT service icinga2 start && service grafana-server start && service apache2 start && bash
    
Now it's up to you doing some stuff. Have fun!