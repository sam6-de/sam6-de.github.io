---
layout: post
title:  "Devoxx 2015 notes: unRESTful Web Services with HTTP/2"
date:   2015-11-13 10:45:00
categories: blog
share: true
comments: true
tags:
- rest
- unrestful
- http/2
- web service
---

*Talk by Fabian Stäber (ConSol Software GmbH)
    #Devoxx #unrestful
    <https://github.com/fstab/h2c>
    @fstabr*

###Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/wR3o6HA47Ao" frameborder="0" allowfullscreen></iframe>

###Slides
<script async class="speakerdeck-embed" data-id="af1c180f9ccc46ffb2fe063539ccf8e0" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

### Overview
HTTP/2, the new version of the HTTP protocol, provides a lot of new features for server-to-server communication:

- bidirectional communication using push requests
- multiplexing within a single TCP connection
- long running connections
- stateful connections

HTTP/2 does not define a JavaScript API, so JavaScript clients running in a Web browser can make only limited use of the new capabilities. However, for server-to-server communication, HTTP/2 provides a lot of ways to go beyond existing REST APIs.

This talk shows HTTP/2's potential for new server-to-server APIs.

###HTTP/2 Intro —> My first HTTP/2 Request
###HTTP/2 new features and how to use them in Java
- multiplexing
- server push
- Stream prioritization

##Testing Http/2-based REST services

###HTTP/2 is already there!

###Why HTTP/2?
####reducing latency
- less protocol overhead
- header compression
- multiplexing
- prio

###HTTP/2 does not break the Web
- goal: any http/1 app can be deployed on http/2 server without code change
- same semantics as http/1
    - resp/req based communication
    - http sessions and cookies

##SSL required (but debug mode works without)
###ALPN Application Layer Protocol Negotiation
- SSL is implemented in the JRE
- Java has to be started with -Xbootclasspath/p:alpn-boot-VERSION.jar
- ALPN will be part of Java 9 with JEP 244

###TOOLS
- OkHttp is a http/2 enforced client
- Netty
- Jetty http/2 client library
- upcoming standard libs:
    - Java SE: HttpUrlConnection JEP 110
    - Java EE: Jax-RS Client API (???)

###Server Push
- Request sent from client to server
    - actually: push promise sent from server to client
##http/2 is not a replacement for WebSockets(!)

###PUSH and REST:
- REST interface where client polls a resource
- in case server push is supported, client polls not by tune interval but polling is triggered by push message
- fully compatible with REST semantics but much more efficient than polling with http/1
- makes workarounds like long polling, websockets, etc. obsolete

###Prioritization
- HEADERS frame may contain stream dependency
- PRIORITY frame contains weight field
- API planned in Servlet 4.0

###flow control
- based on WINDOW_UPDATE frames
- Send up to n bytes, then wait for WINDOW_UPDATE

###Testing
- rhuss-docker-maven-plugin
    - org.jolokia:docker-maven-plugin
- OkHttp MockWebServer
    - for http/1 and http/2 (using ssl library)
    - add bootclasspath alpn as argLine to the maven-surefire-plugin

###Links:
- [hpbn.co](http://hpbn.co)
- <https://http2.golang.org/gophertiles>
