---
layout: post
title:  "Devoxx 2015 notes: Refactor your Java EE application using Microservices and Containers"
date:   2015-11-10 09:30:00
categories: blog
share: true
tags:
- microservices
- docker
- container
---

*Talk by Arun Grupta (@Couchbase; [blog.arungrupta.me](https://blog.arungrupta.me))*

###Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/muqOStp3A_o" frameborder="0" allowfullscreen></iframe>

###Slides
[https://github.com/arun-gupta/microservices/raw/master/slides/refactor-microservices.pdf](https://github.com/arun-gupta/microservices/raw/master/slides/refactor-microservices.pdf)

### Overview
Microservices allow to decompose a monolithic application into cohesive and multiple decoupled services. Each service is running in its own process and communicate using lightweight mechanisms, such as HTTP API. These services are built around business capabilities or functional decomposition. Microservice also enables true polyglot architecture – both in terms of language and data. It truly allows you to employ the right tool for the right job. Each service is independently deployable and lends very well to fully automated deployment machinery.

Can you take an existing Java EE application and decompose it into microservices? What tools are required to enable CI/CD? How does it enable polyglot? What are different design patterns for microservices? What options are available for Service Discovery, distributed logging, load balancing? What tools do you need to manage such services? Is the complexity being pushed around from service implementation to orchestration?

This session will explain how to refactor an existing monolith into microservices and the complexities and benefits it introduces.

### Monolith application
- UI
- Database
- Business Logic

### Advantages
- typically packaged in a single .ear
- easy to test (all required services are up)
- simple to develop

###Version management
- complete packaging of entire application

###Disadvantages
- difficult to deploy and maintain
- obstacle to frequent deployments
- dependency between unrelated features
- makes it difficult to try out new technologies/frameworks

###The art of scalability (martin l. abbott, Michael t. fisher)
- 3 tiers of scaling, e.g. sharding

Photo: definition of micro service used by RedHat

*„Microservice is not a free lunch“*

Netflix Hystrix -  defend your app?!?!


###Characteristics
- Build your teams around business capability (8-10 people)
- single responsibility principle: do 1 thing
- explicitly published interface
- independently replace and upgrade
- with great power, comes great responsibility: „you build it, you run it“
- Designed for failure: „Fault tolerance is a requirement, not a feature“
- „pick the right tool for the right job“ (do not use weblogic)
- 100% automated
- Sync vs. Async Messaging
    - Thrift
    - Protobuf
    - Avro ?!?!?!
    - REST
    - Queue
- Smart endpoints, dumb pipes
- SOA 2.0
    - Microservices = SOA -ESB -SOAP -Centralized governance/persistaence -Vendors +REST/HTTP +CI/CD +DevOps +True Polyglot + Containers + PaaS
    - Conway’s Law ?!?!
    - Service Discovery
    - Immutable VM

###Strategies for Decomposing
- Verb or usecase - e.g. Checkout UI
- Noun - e.g. Catalog product service
- Bounded context
- Single Responsible Principle - e.g Unix utilities

####Each Microservice has its own databases
####It fully owns the context

###Refcard 215
- [https://github.com/arun-gupta/microservices](https://github.com/arun-gupta/microservices)
- [https://dzone.com/refcardz/getting-started-with-microservices](https://dzone.com/refcardz/getting-started-with-microservices)

###ELK ?!?!
- [http://blog.arungupta.me/getting-started-elk-stack-wildfly/](http://blog.arungupta.me/getting-started-elk-stack-wildfly/)
- [https://github.com/arun-gupta/elk](https://github.com/arun-gupta/elk)

###Livecoding
- @ZooKeeperServices
- @SnoopServices
- @FixedServices
- CuratorFramework?!?!?!

###Service Registry/Discovery
- Curator
- ZooKeeper
- kubernetes
- etcd
- consul
- osgi
- snoop

###Design Considerations
- UI Fill stack
    - client-side composition
    - Server-Side HTML generation
    - One Service, one UI
- REST Services
- Event sequencing instead of 2PC
- API Management

###API Management
- Centralized governance policy configuration
- Tracking of AOPs and consumers of those APOs
- Easy sharing and discover of APIs
- Leveraging common policy configuration across different AIPs
    - security, caching, rate limiting, metrics, billing, …

###NoOPS
- service replication (Kubernetes, Fleet, Swarm, …)
- dependency resolution (Nexus)
- Failover (Circuit Breaker; Distributed applications!)
- Resiliency (Circuit Breaker)
- Service monitoring, alerts and events (ELK)

###Data Strategy
- private tables-, schema- or database-per-service
- No sharing tables
    - only one service writes to a table, read-only by others
- No transactions across databases
- Logical transactions in application

###Event Sourcing
- State of the application is defined by a sequence of events
- events are stored in a document format
- events are immutable
    - Delete is implemented as an event

###CQRS ?!?!?!
- Traditional: CRUD using same data model
- Command Query Responsibility Segregation
    - different model to update and read information
    - Command: change the state of system, do not return
- multiple writes, less read, cached

###@WiX: Monolith -> Macroservices (4-5) -> Microservices

###Docker-Compose!!!!

###kumuluzEE?!?!?!
- lightweight Framework for JEE Microservices
- uses standard JEE
- produces standalone JARs
- completely modular
- dependency driven
- pick and choose JEE components
    - even their implementations
- JAR includes all dependencies
- ideal for PaaS and Docker

###Comoonents
- Containers
    - Jetty
    - Tomcat
    - Undertow
    - Grizzly
- JEE Containers

###ALL MICROSERVICES STATELESS!!!!!

###How to Scale?
- only uses JSE
- multiple instances and load balance
- scale via PaaS, etc.

###Github-Branch kumuluzee!!!!!
- Slower ROI, to begin with
- container buddy ?!?!?! by joint
- cloudflare ?!?!?!

###*„Cache is an optimization, NOT the solution“*
