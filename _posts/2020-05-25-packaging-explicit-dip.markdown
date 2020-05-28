---
layout: post
title:  "Clean AWS Development: Packaging for Explicit Dependency Inversion"
date:   2020-05-25 09:06:07 -0700
categories: Java, Clean Code, AWS, JavaScript, Dependency Inversion Principle
comments: true
---

For our first step towards a Clean AWS project we are going to start with the packaging of the code.  The dependency inversion principle is maybe the most important [Clean Architecture](CA_REFERENCE) principle when it comes to the maintenance, and testability of code.  Things like version management, conflicts, and dependency explosion can all become thorny issues if the dependency graph of a project is one big tree.

The easiest way I've found to maintain the dependency inversion principle throughout the code base is to make it explicit through packaging.  The packaging for the project will consist of the following: Common, Core, Adapters, Persistence, AdaptersImpl, PersistenceImpl, and Head.   So what do these packages mean? What do they do? What is the purpose?  These are all good questions and I'll tackle each package one at a time.

####Common
Common is almost the same thing as domain model.  It's like a more pragmatic Domain package.  In Common you will have your your domain model, requests and responses from actions within the Core verticals, and any utilities or enums that are useful to have available throughout the code base.  This is considered the most abstract of all of the packages.  There is no logic in this portion of the code base save for methods on the domain classes needed to use the objects effectively.  Common has no dependencies from any other code in your project, unless it is an even more general Common, like something that is used org, or company wide.  

*Dependency List* [empty]

####Core
Core is all of the business logic that exists within a specific service.  It could be called Core, or Engine, both terms are correct, this is what is going to be driving whatever business use case there is have for developing the project.  Core will depend on the models, and interfaces from Common, and on the interfaces from both Adapters and Persistence.  Core is sort of like the intellectual property, or the "special sauce" of a project.  It contains virtually all of the logic that occurs within a program.

*Dependency List* 
 * Common
 * Adapters
 * Persistence

####Adapters
Adpaters packages are where the magic happens!  This package along with the Persistence package are what builds the "moat" around Common and Core, allowing them to be freely developed. Code in the Adapters package is also on the highest level of abstraction and thus has no dependencies. Think of the Adapters package as wrapper interfaces for anything external that the code base needs to interact with. It could be auth, or another service, or anything really.  Wrap the external code client with an interface defined in your Adapters source code.  Then define methods on the interface for every use case you have with the external code.  This does not need to be a one to one wrapping, you may execute multiple calls to the service or even wrap multiple services in a single adapter.  This is you defining the language that your program will use to "speak" with another program.  It is the dependency inversion principle happening in real time.  The packages and this separation of Adapters and AdaptersImpl are what is going to keep your code base clean, easily maintainable, and testable.  This is because the Core package will only use the interfaces defined in the Adapters package so all of the extraneous dependency management can be pushed out to a later date and happen in fewer packages.  

Looking at a diagram of the directed graph that this creates, the Adapters package becomes a sort of connection sink. A node where multiple packages depend on it, but it has no outgoing dependencies.  That breaks the connectedness of the dependency graph from a single rooted tree, into multiple dependency trees that can be compiled separately, independently and with fewer conflicts.  This in effect creates less legacy code, with legacy code being that which is defined as: "something I don't like working with because its difficult for reasons that aren't due to the problem being solved." and is the actual implementation of the dependency inversion

*Dependency List* [empty]

####Persistence
Persistence packages are where the magic happens! The Persistence package(s) are just special implementations of the Adapters package(s).  They really are the same, it's just that persistence is ubiquitous enough, and often central enough to a project that Persistence gets peeled off into its own separate package to not bloat the Adapters package.  Here you will define interfaces for your persistence layer.  The interfaces define the logical actions that the persistence layer will need to be able to execute, think save, batchSave, list, etc.

Persistence is really near and dear to me because I view it as one of the fundamental mistakes engineers have made, myself included.  Often times people see data as the central theme of a large program, and it is, but the actual storage of that data should actually not be a central theme and should not be developed first.  I would argue to push persistence implementation work off as long as possible because it is difficult to undo.  It really helps to view databases as just another [specialized] service, and to treat them as such, like evaluating between two third parties to handle Auth.

*Dependency List* [empty]

####AdaptersImpl
AdaptersImpl is the other half of the 

*Dependency List*
 * Adapters
 * Whatever client that needs to be interface with.

####PersistenceImpl

*Dependency List*
 * Persistence
 * Whatever database client that needs to be interfaced with.

####Head
The Head package brings everything together, it is the "main" function of the project, the entry point, you name it.  Head could be an api, or a website, or batch processing workflow.  It is mainly comprised of dependency injection classes and modules and step function workflows for tying together the actions of core.  Anything else in the Head package is for supporting the step function workflows by giving them permissions etc.  The dependency injection is what gives the interfaces for adapters and persistence some legs by providing the implementation classes anywhere the interfaces are called for.  



Were I making a project for rating dogs it would be DogRaterCommon, DogRaterCore, DogRaterAdapters, DogRaterAdaptersImpl, DogRaterPersistenceImpl, and DogRaterHead.

What is strongly connected graph?

What do we want it to look like? build a moat


