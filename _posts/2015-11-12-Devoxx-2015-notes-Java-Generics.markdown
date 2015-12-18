---
layout: post
title:  "Devoxx 2015 notes: Java Generics: Past, Present and Future"
date:   2015-11-12 16:40:00
categories: devoxx2015 generics
---

*by Richard Warburton 
    @richardwarburto
    [insightfullogic.com](http://insightfullogic.com)*
    
*by Raoul-Gabriel Urma
    @raoulUK*

###Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/LEAoMMEIUXk" frameborder="0" allowfullscreen></iframe>

###Slides
<iframe src="//de.slideshare.net/slideshow/embed_code/key/RcvhM1q2cMXZL" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//de.slideshare.net/RichardWarburton/generics-past-present-and-future" title="Generics Past, Present and Future" target="_blank">Generics Past, Present and Future</a> </strong> from <strong><a href="//de.slideshare.net/RichardWarburton" target="_blank">RichardWarburton</a></strong> </div>

### Overview
Generics are one of the most complex features of Java. They are often poorly understood and lead to confusing errors. Unfortunately, it won’t get easier. Java 10, release planned for 2018, extends Generics. It’s now time to understand generics or risk being left behind.

We start by stepping back into the halcyon days of 2004 and explain why generics were introduced in the first place back. We also explain why Java’s implementation is unique compared to similar features in other programming languages.

Then we travel to the present to explaining how to make effective use of Generics. We then explore various entertaining code examples and puzzlers of how Generics are used today.

Finally, this talk sheds light on the planned changes in Java 10 with practical code examples and related ideas from other programming languages. If you ever wanted to understand the buzz around primitive specialisation or declaration site variance now is your chance!

###Past
- in 2004
    - thefacebook started
    - java added generics
        - compile error instead of runtime error
    - Triangle: Simplicity - Static Safety - Concision

###Present
- in 2015
    - Intersection types
        - <T extends A> = T has is a subtype of A
        - <T extends A & B> = T is a subtype of A AND B
    - curiously recurring generics pattern
        - serializable lambdas
    - wildcards

###Future
