---
layout: post
title:  "Clean AWS Development: Hello World!"
date:   2020-05-25 09:06:07 -0700
categories: CleanAWS Hello
comments: true
---

This is the first, of hopefully many posts that will outline how to develop Clean AWS projects.  Much of the philosophy is borrowed from some combination of [Clean Code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882), [Clean Architecture](https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164), [Domain Driven Design](https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215), [Growing Object Oriented Software Guided by Tests](http://www.growing-object-oriented-software.com/), and [How to Take Great Notes](https://takesmartnotes.com/).  Throughout these rants I'll point the reader to other resources I've found to be valuable such as [Amazon Web Services in Action](https://www.amazon.com/Amazon-Services-Action-Andreas-Wittig/dp/1617292885), and some of [acloud.guru](https://acloud.guru/)'s courses.  

The posts are meant to help enterprise engineers who need to scale development.  I'd imagine that anyone who's programming could find them helpful though.  The ability to code cleanly at all levels of the stack, from your infrastructure as code, to your services, to your UI significantly speeds up development, but also, even more importantly, allows you to free your mind from having to keep too much in it so that you can think about the problem at hand.  

The stack I will use for examples is some combination of Java, CDK, Dagger, and AWS Serverless development resources.  I chose Java mainly because it is ubiquitous and though verbose, easily expresses all of the ideas needed for Clean AWS Development.  I also think that internet tech blogs seem to be an echo chamber for JavaScript development, or really just any dynamically typed language where the main focus is "look how few lines I can do this in".  Yes, less code is sometimes better, but also sometimes more code reveals intent, separates concerns, and allows future engineers to understand what is actually going on. Also explicit types are good.  Full disclosure, JS was what I cut my teeth on and I love it but unless you're using TypeScript I don't believe its that suitable for large scale enterprise development, it's just too hard to get it right.

The initial posts will be about code, code packaging, code styles, and how to set up development to make the process of actually getting to production in a sane manner work.  After all, architecture all starts with code, if it's not set up right it's going to be a pain no matter what!