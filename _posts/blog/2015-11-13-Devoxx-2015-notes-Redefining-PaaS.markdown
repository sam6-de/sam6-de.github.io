---
layout: post
title:  "Devoxx 2015 notes: Redefining PaaS: Managed container based microservices on Google App Engine"
date:   2015-11-13 11:50:00
categories: blog
tags:
- paas
- cloud
- microservices
- container
---

*Talk by Ludovic Champenois, Amanda Waite (Google)
    @ludoch, <https://plus.google.com/+ludovicchampenois>
    @tekgrrl, <http://developers.google.com>
    @googlecloud*

###Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/aKUlu9-psZo" frameborder="0" allowfullscreen></iframe>

### Overview
Microservices are a great model for developing modern applications. But microservice based architectures are more complicated than traditional monolithic apps and can come with a significant management overhead particularly when you need to maintain different environments for dev, test and production. This overhead either falls on your development team or to people you hire specifically to manage it.

This talk with multiple demos, looks at how to build, test and deploy container based microservice architectures without having to worry about managing the platform on which they run, thus freeing up your developers to do what they do best, writing application code.

We'll look at new and enhanced features of Google App Engine and walk through the staged deployment of both a simple application and a more complicated microservices based architecture. We'll also look at how App Engine has evolved from it's original sandboxed runtime environment to what it is today, a next generation platform for building and deploying modern day applications.

###Goal
- scale up to serve more traffic

###Microservices way?
- Package and deploy
    - helloworld-service,jar
    - guestbook-service.jar
    - app.jar
- architectural style
- decompose large app
- each part has own realm of responsibility
- one micro service can call many other services

###goals
- define strong contracts
- allow independent deployment cycles including rollback
- easy to deploy and version
- facilitate a/b testing concurrently on subsystems
- Link: http://microservices.io
- improve clarity of logging and monitoring
- provide fine-grained quota and cost accounting
- increase aberall app scalability and reliability
- expect failures at any time
- evolutionary design
    - small changes
    - easier testing

###Embedded java servers
- app engine sandbox
- spring boot,
-  vert.x s
- park
- ninja
- fluent http
- jodd
- jboss wildfly swarm
- jetty9

###compute on cloud platform by Google
- app angine
- container engine
- compute engine

### *„compute for every need“*

###What GAE can provide for micro services?
- GAE Sandbox
    - highly scalable PAAS with
        - java
        - python
        - go
        - php
        - java7
        - java8 coming
- GAE Managed VMs
    - java8
    - python
    - go
    - node.js
    - Docker on our own runtime
- App Engine project ist multi modules capable, some sandbox, some VMs
- cane scaled independently
- can be implemented by any runtime
- versionable
- traffic splitting between versions
- Project items share the same Datastore, memcache, task-queue, …
hint: module==service

###Inter module communications by modulesAPI from google (see photo)

###New GAE Java managed VMs runtimes
- docker images hosted on grr.io
- pure openjdk8
    - simple app.yaml
- pure Servlet 3.1
    - simple app.yaml
- for GAE Sandbox apps:
    - Jetty9 + Sandbox API compatibility layer
        - run GAE sandbox applications as is
        - needs appengine-web.xml

###New tooling
- Cloud SDK
    - fingerprinting
    - deploy app.yam condign
        - cloud preview app deploy app.yaml
- Maven plugin for cloud SDK
    - Automatik stating phase for GAE Sandbox combat applications
    - or out app.yam and Dockerfile

###API Design
- microservices on app engine all one another
- micro services deployed and maintained independent of one another
- easily addressable within a project or inter projects

###Links
<http://www.grpc.io>
