---
layout: post
title:  "Devoxx 2015 notes: Beyond the CAP theorem, consistency without consensus with CRDTs"
date:   2015-11-11 16:40:00
categories: blog
share: true
comments: true
tags:
- database
- cap theorem
- crdt
- consistency
---

*Talk by Sam Bessalah (about.me/bessalah) @samklr*

###Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/d8GJMyIGhLw" frameborder="0" allowfullscreen></iframe>

### Overview
Building distributed systems is about making trade offs. One example of trade off is sometimes having to choose between availability and strong consistency, like stated in the famous CAP Theorem. In real life production systems, providing predictable eventual consistency in highly available distributed systems is often done via external synchronization mechanisms, and complex consensus protocols like Paxos, RAFT, 2PC or Zab (Zookeeper).

Hopefully, a lot of academic research to provide strong eventual consistency has emerged in the form of Commutative and Convergent Replicated Data Types, commonly known as CRDTs. CRDTs are data structures that look similar to well-known data types like counters, sets, or maps. But their internal structure makes them safe for concurrent and distributed updates, without requiring any consensus between writers, and without any loss of information in the face of concurrency.

###Other talk
- [https://t.co/Klkz5F29a8](https://t.co/Klkz5F29a8)

###Eventual consistency without consensus with CRDTs

###Why distributed system?
- avail
- fault tol
- throughp
- architecture
- economics

###CAP Theorem
- Data consistency?
    - consensus systems, locking services and barbaric algorithms
    - Distributed com census
    - 2-phase-commit
    - 3-phase-commit
    - paxos
        - not the island ;-)
        - Leslie Lamport
    - RAFT
        - „simplified“/„understandable“
    - locking services
        - chubby (google)
        - Zab (yahoo)
- „consistency is a property of your data, not of your nodes“
- AP Systems Enebtual Consistency

###Dynamo Systems (P2P-Systems)
- Riak
- Voldemort
- Cassandra
- HA

###Conflict resolution
- semantic resolution
- vector clocks
- LWW Last write wins

###Convergent (and commutative) replicated data types
- from INRIA (Paris)
- CRDTs
    - take consistency prob to level of data structures
    - their state resolves automatically (eventually) to…
    - Monotonicity
    - 2 types:
        - state based CvRDTs
            - all reps connected
            - at least once semantics usually
            - state changes advance upwards according to partial order
        - commutative CmRDTs
            - all replicas connected
            - need reliable nrpwdcast with ordering guarantees
            - best suitable for commutative updates
        - Fancy words:
            - idempotent
            - commutative
            - monoids
        - aka: Joint semi lattice
    - PN-Counter (used for grow-only)
    - G-Set
        - grow only set, that only allows to add an element
    - 2Phase set
        - build of 2 G-Sets for add and removal
        - has a tombstone that maintains deleted elements (—> Elements can’t be re-added)
    - LWW-Element-Set
        - timestamp to add and remove states
        - greatest timestamps wins
        - close to cassandra model

###CRDTs in the wild
- „call me maybe“
- [http://jepsen.io](http://jepsen.io)
    - riak
    - cassandra
    - Roshi [https://github.com/soundcloud/roshi](https://github.com/soundcloud/roshi)

####RAFT

####PAXOS

####Speaker uses Kafka

