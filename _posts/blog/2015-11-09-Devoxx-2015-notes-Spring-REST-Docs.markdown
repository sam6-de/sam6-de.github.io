---
layout: post
title:  "Devoxx 2015 notes: Spring REST Docs - Documenting RESTful APIs using your tests"
date:   2015-11-09 17:25:00
categories: blog
share: true
comments: true
tags:
- spring
- rest
- documentation
- test
---

*Talk by Andreas Evers (Ordina Belgium)*

### Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/iWj-t69EdN4" frameborder="0" allowfullscreen></iframe>

### Slides
<script async class="speakerdeck-embed" data-id="5c34fae7838844d4b565c01f8b06d31c" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>

### Overview
Writing documentation for truly RESTful APIs is often done by Swagger or similar tools. However, they provide no support for documenting hypermedia elements, they are URI-centric, leaky and intrusive.

Wouldn't it be nice if your documentation could fail your build in case it gets out-of-date with your production code? Wouldn't you want to write your documentation using a format which is designed for writing? Can't there be a tool which documents your code without using the actual implementation?

Meet Spring REST Docs.

REST Docs allows you to write your documentation as part of your integration tests. In case your documentation doesn't match with your integration test result, your test will fail. This ensures accurate documentation while avoiding the need to put annotations on your production code. It uses the same integration tests to generate snippets, both for request and response bodies, and for hypermedia links. As a writing format it uses asciidoc which is designed for writing.

Follow this session to get a demo on how this tool simplifies your life, and how easy and natural it is to use.

###Spring REST Docs
- no doc is better than wrong doc
- should be: easy to write, format: designed for writing
- Framework should not analyse implementation to predict documentation

###swagger?!
- doesn’t support hypermedia

###target
- avoid more annotations

####integration tests -> generated snippets + manually written template -> generated html

###conclusion
- no annotation -> use test classes/methods by using .andDo(document(….

###snippets and template are generated/written using asciidoc

###Features
- packaging into jar
- hypermedia links
- payloads
- request/path parameters
- header
- customisable
- ignore parts
- mark as optional
- JSON and XML
- Upcoming: REST Assured
- prettyPrint()

### REST Assured?!?!?

###brownfield stubbing?!

###Links
- <http://projects.spring.io/spring-restdocs/>
- <https://github.com/spring-projects/spring-restdocs>

###Demo application
<https://github.com/andreasevers/spring-restdocs-demo-devoxx>
