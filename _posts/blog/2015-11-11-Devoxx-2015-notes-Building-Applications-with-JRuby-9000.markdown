---
layout: post
title:  "Devoxx 2015 notes: Building Applications with JRuby 9000"
date:   2015-11-11 14:00:00
categories: blog
tags:
- jruby
---

*Talk by Charles Oliver Nutter (RedHat, @headius; Ruby, Mirah, JNR)*

###Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/1t4iu7tefi4" frameborder="0" allowfullscreen></iframe>

###Slides
<iframe src="//www.slideshare.net/slideshow/embed_code/key/1RWLIX0yuIcgDd" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/CharlesNutter/jruby-9000-taipei-ruby-users-group-2015" title="JRuby 9000 - Taipei Ruby User&#x27;s Group 2015" target="_blank">JRuby 9000 - Taipei Ruby User&#x27;s Group 2015</a> </strong> from <strong><a href="//www.slideshare.net/CharlesNutter" target="_blank">Charles Nutter</a></strong> </div>

### Overview
Ruby has come a long way since JRuby first ran Rails in 2006. Frameworks like Rails have grown up with the modern web, now supporting web sockets, microservices, and integration with Javascript client libraries like Ember. Concurrency utilities modeled after the JDK are helping Ruby scale horizontally. Applications can be built with Rake - or with JRuby plugins for Gradle and Maven. Maven poms can be written in a beautiful Ruby DSL. Swing, JavaFX, and other graphics libraries become easy and fun with JRuby. Sass and Asciidoctor are already being used in Java apps thanks to JRuby. And you can bundle up the whole thing in an executable jar or war file; your devops will never know it's Ruby. Come see what JRuby 9000 can do for you in 2015.

###Ruby from 9000 feet
- ruby on jam
- eclipse public license (or GPL/LGPL)
- Solid Java integration

###Ruby? People still use that?

###Ruby community
- strong, independent, creative, and fun
- rubygems.org

###TIOBE language index ?!?!?!

###GitHut ?!?!?!

###Sass
- external DSL for CSS
- came from ruby

###Cucumber
- story based testing

###Asciidoctor
- fast text processor
- to html5
- a ruby gem, usable from java

###JRuby Community?

###Ruby Quick Tour
- example Java vs. Ruby
- Duck Typing
    - obj.swim()
    - duck.swim()
    - anchor.swim()
- similar danger to Java typecasting
- testing eliminates vast majority of cases
- rarely an issue for real Ruby applications

###Embedding Ruby
- Example: Purugin Plugin for Minecraft (https://github.com/enebo/Purugin)
- Minecraft + Spigot
- egg madness plugin (create 100 chickens for each thrown egg)

###Build tools
- Rake (standard)
    - most popular
    - modeled after make
    - rake.rubyforge.org
    - can use regular ruby code, e.g. delete all files with white-/blacklist
- Maven
    - verbose XML
    - 100% declarative
    - weak set of defaults, conventions
- Maven Polyglot
    - multi-language-support
    - Ruby build uses Ruby support
    - https://github.com/takari/polyglot-maven
- maven-wrapper
    - https://github.com/takari/maven-wrapper
- JRuby/Gradle
    - jruby-gradle.org

###Testing with Ruby

###Web Applications
- Rails
    - original full-stack framework
    - convention over configuration
    - still the best way to build web apps quickly
- the rails way
    - all apps have same layout
    - app self-contained
    - many simple dev commands
    - run directly
    - scaffolding
- Micro
    - trend away from monolithic app
    - rack
        - subys servlet api
    - Sinatra
        - micro web framework
    - Bundler
        - like pom.xml only for dips
        - separate dev, test, prod
    - Gefiel
    - Torquebox
        - torquebox.org
        - Ruby-flavored JavaEE
        - example: all dips in a single executable jar file
    - Warbler
        - creates executable war file
    - Sneak Ruby into the enterprise
