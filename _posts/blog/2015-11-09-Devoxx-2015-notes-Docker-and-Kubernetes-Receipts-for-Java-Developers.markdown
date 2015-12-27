---
layout: post
title:  "Devoxx 2015 notes: Docker and Kubernetes Receipts for Java Developers"
date:   2015-11-09 09:30:00
categories: blog
tags:
- docker
- kubernetes
comments: true
share: true
---

*Talk by Arun Gupta (Couchbase)*

<iframe width="560" height="315" src="https://www.youtube.com/embed/aSATsLG59Zs" frameborder="0" allowfullscreen></iframe>

### Overview
Containers are enabling developers to package their applications in new ways that are portable and work consistently everywhere. Docker has become the de facto standard for those portable containers in the cloud. Kubernetes provides an open source orchestration of Docker containers. This tutorial will provide an introduction to Docker and Kubernetes. The talk will explain several recipes on how to create and publish Docker images that package Java EE applications. Design patterns and anti-patterns that show how to create cluster of such applications will be shown. Replicating your environments using Docker images will be shown. Attendees will learn how Kubernetes’s self-healing mechanism can be used to create cluster of these containers.

* docker machine (Mac and Windows)
* (not yet) for production use

### Providers
* GCE Plugin
* EC2 plugin
* DigitalOcean DO plugin
* Azure plugin
* Rackspace plugin

### boot2docker
migrate to docker machine:
docker-machine create -d virtual box —virtual box….

### Docker toolbox:
* Docker compose (Mac)
* docker client
* docker machine
* docker kinematic
* boot2docker ISO
* virtualbox

### Livecoding
* docker-machine ls
* docker-machine —help
* docker-machine create -d=virtualbox devoxx
* cd ˜/.docker/machine/cache -> ISO
* cached copy of boot2docker
* docker-machine env devoxx
* docker build -t helloworld-devoxx . (into docker file directory)
* docker images
* !!!USE BUSYBOX!!! even in production
* docker run helloworld-devoxx
* docker run -it ubuntu sh
* docker ps -a
* docker run -it -p 8080:8080 jboss/wildfly
* eval $(docker-machine env devoxx)

### Docker Compose
* docker compose instead of linking containers

### Multi-Host Networking
* overlay network (multiple hosts) instead of bridge network spans (can only be used for single hosts)
* works with swarm (experimental in compose)

### DEV/PROD with Compose
* 2 files in one directory, 1 for dev, 1 for prod

### Docker Swarm
* native clustering
* fully integrated
* serves standard Docker API
Stress tested 1000 EC2-Nodes, 30k containers, worked.

### Zookeeper alternatives:
* etcd
* consul

### Node Discovery
docker-machine create
—engine-opt—

### Master HA
* handle failover of manager instance
* primary manager, multiple replica (and proxy these to primary)
* docker run swarm manage
    * —replication
    * —replication-ttl
    * —advertise
    * —addr

### Scheduling Backends
* CPU -c
* RAM -m
* docker-machine create -strategy <value>
    * spread (default)
    * binpack
    * random
* API backend (e.g. Mesos) coming

### Persistent Storage
* docker volume —help
* Plugins: Flocker, Ceph, GlusterFS…
* docker volume create …

### Livecoding
* docker-compose logs
* docker network ls (3 default networks)
* docker-compose —x-networking up -d (EXPERIMENTAL, NOT FOR PRODUCTION USE)
* creates a bridged network „tmp“

### Kubernetes
* Kubernetes Master
* KubeDNS
* KubeUI
* Default: Crestes 1 Master, 1 Minion
* Kubernetes Provider: e.g. vagrant

### Commands
* kubectl get pods pr minions
* kubectl create -f <filename>
* update
* delete
* resize (ReplicationController)
* …

### Services
* abstammt set of pods aas a single ip and port
* creates env vars in other pods
* stable endpoint for pods to reference (DNS routing/DNS mod)

### Replication Controller
* eures that a specified number of pod replicas are running
    * templates cookie cutters
    * rescheduling
    * manual auto-scale replicas
    * Rolling updates
* recommended : Pod or Service in a RC
* restart-policies (e.g. always)

### Health Checks
* performed by Kubernetes

### kubernetes using docker
* http://kubernetes.io/v1.0/doca/gettingstarted-guides/docker.html

### Links
* https://de.wikipedia.org/wiki/AsciiDoc
* https://htmlpreview.github.io/?https://github.com/javaee-samples/docker-java/blob/master/readme.html
* https://github.com/javaee-samples/docker-java/blob/master/chapters/docker-vs-kubernetes.adoc
* https://github.com/javaee-samples/docker-java/



