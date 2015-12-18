---
layout: post
title:  "Devoxx 2015 notes: Software architecture as code"
date:   2015-11-13 09:30:00
categories: devoxx2015 saac
---

*by Simon Brown*

### Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/oDpdaXt0HQI" frameborder="0" allowfullscreen></iframe>

### Slides
<iframe src="//www.slideshare.net/slideshow/embed_code/key/9nB8qW8x1emdjZ" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/MozaicWorks/simon-brown-software-architecture-as-code-at-i-take-unconference-2015" title="Simon Brown: Software Architecture as Code at I T.A.K.E. Unconference 2015" target="_blank">Simon Brown: Software Architecture as Code at I T.A.K.E. Unconference 2015</a> </strong> from <strong><a href="//www.slideshare.net/MozaicWorks" target="_blank">Mozaic Works</a></strong> </div>

### Overview
Over the past few years, I’ve been distilling software architecture down to its essence, helping organisations adopt a lightweight style of software architecture that complements agile approaches. This includes doing "just enough" up front design to understand the significant structural elements of the software, some lightweight sketches to communicate that vision to the team, identifying the highest priority risks and mitigating them with concrete experiments. Software architecture is inherently about technical leadership, stacking the odds of success in your favour and ensuring that everybody is heading in the same direction.

But it’s 2015 and, with so much technology at our disposal, we’re still manually drawing software architecture diagrams in tools like Microsoft Visio. Furthermore, these diagrams often don’t reflect the implementation in code, and vice versa. This session will look at why this happens and how to resolve the conflict between software architecture and code through the use of architecturally-evident coding styles and the representation of software architecture models as code.

### Notes
- Abstraction
- The „4+1“-View model
- Brain freeze!
    - would we code it that way?
    - did we code it that way?
- A common set of abstractions is more important than a common notation
    - diagrams are maps that help a team navigate a complex codebase
- Software system
    - 1-n contaners
        - 1-n components
            - 1-n classes
- Static model
    - data
    - runtime and behavior
    - Business process and workflow
    - …
- The C4 model
    - System context
    - Containers
    - Components
    - Classes
- Devs are most important stakeholders of software architecture

###Tools
- Stencils? Old school way
- Diagramming tools see packages and classes rather than components

###Step back
- What is a component?
    - Spring PetClinic (see github)
    - https://speakerdeck.com/michaelisvy/spring-petclinic-sample-application
    - What are the architecturally significant elements?
        - better UML diagram
    - Next step: bundling
- The code is the embodiment of the architecture
    - Q: Is the architecture in the code?
    - in practice. architecture is embodied and recoverable from code, and many languages provide architecture level views of the system. (Paul C. Clements)

###„Modular Monoliths“ ?!?!?!

### *„Extract as much of the software architecture from the code as possible, and supplement where necessary“*

—> Create an architecture description code using Java
- example using PetClinic using C4-model

###TOOL
Structurizr for Java on github

### [techtribes.je](http://techtribes.je) architecture as a graph (AaaG)


