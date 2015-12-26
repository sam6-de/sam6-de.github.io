---
layout: post
title:  "Devoxx 2015 notes: Six principles for building fault tolerant microservices on the JVM"
date:   2015-11-12 17:50:00
categories: blog
share: true
tags:
- fault tolerance
- microservices
- jvm
- principles
---

*Talk by Christopher Batey (@chbatey)
    <http://christopher-batey.blogspot.co.uk/>*

#Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/dKWNZnuZhd0" frameborder="0" allowfullscreen></iframe>

### Overview
Are you developing applications that communicate over a network? Of course you are! This talk will take you through all the ways you can build fault-tolerant applications and how, once you get your team in the mindset that everything will eventually fail, dealing with the failures gracefully is no more work than building fragile applications.

With supporting slides, I'll cover the theory and motivation behind moving to a more distributed architecture and then go through the pitfalls and the strategies for improving fault-tolerance, backed up with real examples from the system I built at Sky television.

After an introduction the talk will be split into 6 sections/principles/lessons learned from the field:

- Implementing SLAs + timeouts in a networked environment
- Proactively avoiding work: bound your queues
- Failing gracefully + don’t cascade failures
- Circuit breaking unreliable dependencies
- Kill switching unreliable dependencies
- Know it is your fault: monitoring a micro service architecture

###Problem 1: what, if everything crashes
- small horizontal scalable services
    - move to small services
    - move to a horizontally scalable database that can run active-active in multiple data centers
- Half way point
- Tech used
    - all on github
    - techs:
        - dropwizard
        - wiremock
        - Hystrix
        - Saboteur
- microservice def:
    - == no cdi framework
    - testing microservices
        - you don’t know a service id fault tolerant if you don’t test faults
    - The test double
        - wire mock for http integration
        - stubbed cassandra for database
        - Kafka unit
- BOOK: Release it! from Michael T. Nygard
- Isolated service tests

###Fault tolerance
- 1. don’t take forever
    - if at first don’t succeed, don’t take forever to tell someone
    - Timeout and fail fast
        - Which timeouts?
            - Socket connection timeout
            - Socket read timeout
                - Your service hung for 30 seconds :-(
            - Resource acquisition
                - Your service hung for 10 MINUTES :-(
                - adding automated test
                    - vagrant
                    - Saboteur - uses tc, iptables to simulate network issues
                    - Wiremock - mock HTTP dependencies
        - implementing reliable timeouts
            - protect the container thread!
            - homemade: worker Queue + Thread pool (executor)
            - hystrix
            - Spring cloud Netflix
        - Drive this via requirements
            - reliable timeout
            - Throttling
            - Monitoring of failures / successes
        - Timeouts take home
            - you can’t use network level timeouts for SLAs
            - Test your SLAs
            - Scary things happen without network issues
- 2. Don’t try if you can’t succeed
    - Complexity
        - when an applications grows in complexity it will eventually start using queues and thread pools
    - bound your queues and threads
    - fail quickly when the queue / maxPool
    - This is a functional requirement
        - set the timeout very high
        - Use wire mock to add a large delay
        - set queue size and thread pool size to 1
        - Send in 2 requests
        - What happens on the 3rd request?
- 3. Fail gracefully
    - expect rubbish
        - invalid http
        - malformed response bodies
        - connection failures
        - huge/tiny responses
    - Testing all of it with wiremock
    - Stubbed cassandra
- 4. Don’t whack a dead horse
    - What to do
        - yes this will happen…
        - mandatory dependency - fail *really* fast
        - Throttling
        - Fallbacks
    - Circuit breaker pattern ?!?!?!?
- 5. Turn broken stuff off
    - The kill switch

### #FERNIEDEER
