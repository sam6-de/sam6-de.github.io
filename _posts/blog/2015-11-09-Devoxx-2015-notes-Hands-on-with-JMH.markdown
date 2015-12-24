---
layout: post
title:  "Devoxx 2015 notes: Hands-on with JMH, become a benchmarking expert in 30 minutes!"
date:   2015-11-09 16:45:00
categories: blog
tags:
- jmh
- benchmark
---

*Talk by Tom Vleminckx (FIS, <http://www.clear2pay.com>) #Devoxx #Jmh*

###Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/Bi0E7w1ZFFA" frameborder="0" allowfullscreen></iframe>

###Slides
<iframe src="//de.slideshare.net/slideshow/embed_code/key/fKJ1rd8RiSf7uV" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//de.slideshare.net/TomVleminckx1/handson-with-jmh-become-a-benchmarking-expert-in-30-minutes" title="Hands-on with JMH, become a benchmarking expert in 30 minutes!" target="_blank">Hands-on with JMH, become a benchmarking expert in 30 minutes!</a> </strong> from <strong><a href="//de.slideshare.net/TomVleminckx1" target="_blank">Tom Vleminckx</a></strong> </div>

### Overview
In this talk we will introduce the JMH benchmarking framework and walk the attendees through some simple, yet practical examples. We will target raw performance improvements , but also show how the approach helps to hunt down race-conditions and deadlocks in your code. We'll also cover the build-in profiler support and show how to automate the tests from gradle.

### Notes
- Why we should write benchmarks
- JMH
- Write-run-profile-report

###Why?
- Fail fast
- same as Unit-Testing, etc.
- race condition detecting

###Hard, because of
- Multi-Threading
- Mistakes/limits/knowlegde,…

###Better: use JMH
- Java Microbenchmark Harness (JMH)
- JIT crows OpenJDK Tool
- away hard part, @-based

###Write
- Maven archetype
    - or Gradle
    - different JVM languages supported
- @-based
    - code generation
    - @Setup, @Benchmark, @TearDown

###Run
- warmup needed
- threads
- iterations
- multiple modes
    - throughput
    - avg time
    - sample time (percentiles)
    - single shot

<https://t.co/pqP8GjoJYp>

###Want to learn more?
- <http://openjdk.java.net/projects/code-tools/jmh/>
- <http://hg.openjdk.java.net/code-tools/jmh/file/tip/jmh-samples/src/main/java/org/openjdk/jmh/samples/>

###Get the examples of this talk
- <https://github.com/tvleminckx/jmh-devoxx-tia>
