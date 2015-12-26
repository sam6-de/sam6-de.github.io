---
layout: post
title:  "Devoxx 2015 notes: Dockerize user stories with Mayfly"
date:   2015-11-11 15:10:00
categories: blog
share: true
tags:
- docker
- mayfly
- jira
- jenkins
---

*Talk by maarten dirkse (bol.com)
 @mdirkse #Devoxx #mayflycd
 @bol_com_OSS*

###Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/cHU8yEV8FwQ" frameborder="0" allowfullscreen></iframe>

###Slides
<iframe width="560" height="315" src="http://mayflycd.github.io/mayfly-talks/2015-06-25_Mayfly---Dockerize-your-user-stories-indepth_bol.com/#/" frameborder="0" allowfullscreen></iframe>

### Overview
This talk introduces Mayfly, a development platform built by bol.com. Mayfly speeds up your service development by wrapping your scrum user story code in containers, testing it in an isolated, production-like environment and automatically enforcing your Definition of Done.

###introduction:
- Docker-based
- user story centered

###Genesis of Mayfly
- Idea
    - single branch
    - Jenkins
    - production
    - scaling these steps up
    - only thing brakes: broken branch
- User stories are the unit of work
    - support scrum process
    - drive the workflow from Jira
- Give dev team full control
    - Docker
    - feature branches & pull requests
    - Jenkins & JobDSL
- Give every user story an independent runtime environment for development, testing and acceptation
- Done == ready for production

###Mayfly provides per US:
- feature branch in SCP
- CI jobs (jenkins)
- isolated, prod like env
- automated DoD-check
- Logs & metrics (log stash, graphite, prometheus)
- optional US-specific DB

###Demo
- definition of done specified as XML to use in Jira
- creates branches, Jenkins jobs, etc. on the fly using Jira
- everything is cleaned up afterwards

###Used Tools:
- Consul
- marathon
- docker
- mesos
- jira
- Jenkins
- stash
- mongodb
- postgresql
- oracle

###Links
- [https://github.com/mayflycd](https://github.com/mayflycd)
- [mayfly.cd](mayfly.cd)

