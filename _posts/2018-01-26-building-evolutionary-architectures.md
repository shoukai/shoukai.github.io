---
layout: post
title:  "《Building Evolutionary Architectures》读书笔记"
subtitle: "构建进化软件架构 读书笔记"
date:   2018-01-28 8:00:00
background: 'http:/\/oo6gt25nl.bkt.clouddn.com/04.jpg'
---

# Building Evolutionary Architectures
-------
## Chapter 1. Software Architecture
-------
* An initial part of an architect’s job is to understand the business or domain requirements for a proposed solution.
* Here is our **definition of evolutionary architecture**: An evolutionary architecture supports guided, incremental change across multiple dimensions. （进化软件架构定义：演进式体系结构支持跨多个维度的引导性增量变更。）
* There are no separate systems. The world is a continuum. Where to draw a boundary around a system depends on the purpose of the discussion. - Donella H. Meadows
* **Multiple Architectural Dimensions** Here are some common dimensions that affect evolvability in modern software architectures: 
    * Technical: The implementation parts of the architecture: the frameworks, dependent libraries, and the implementation language( s). 
    * Data: Database schemas, table layouts, optimization planning, etc. The database administrator generally handles this type of architecture. 
    * Security: Defines security policies, guidelines, and specifies tools to help uncover deficiencies. 
    * Operational/System: Concerns how the architecture maps to existing physical and/ or virtual infrastructure: servers, machine clusters, switches, cloud resources, and so on. 
* Each of these perspectives forms a dimension of the architecture — an intentional partitioning of the parts supporting a particular perspective.
* An architecture consists of both requirements and other dimensions, each protected by fitness functions
![evolutionary-scope](http://oo6gt25nl.bkt.clouddn.com/evolutionary-scope.jpg)

* An evolutionary architecture consists of three primary aspects: **incremental change, fitness functions, and appropriate coupling**.

## Chapter 2. Fitness Functions
-------

* The word guided（进化软件架构定义中的指导性） indicates that some objective exists that architecture should move toward or exhibit. 

* We use this concept to **define architectural fitness functions**: 
    * An architectural fitness function provides an objective integrity assessment of some architectural characteristic(s). 
    * 恰当功能提供了一些架构特征的客观完整性评估
* The systemwide fitness function is crucial for an architecture to be evolutionary, as we need some basis to allow architects to compare and evaluate architectural characteristics against one another.
![evolutionary-systemwide](http://oo6gt25nl.bkt.clouddn.com/evolutionary-systemwide.jpg)

* A system is never the sum of its parts. It is the product of the interactions of its parts. Dr. - Russel Ackoff
* **Identify Fitness Functions Early.** Teams should identify fitness functions as part of their initial understanding of the overall architecture concerns that their design must support. They should also identify their system fitness function early to help determine the sort of change that they want to support.
* Fitness functions can be classified into three simple categories: 
    * Key（关键）: These dimensions are critical in making technology or design choices. More effort should be invested to explore design choices that make change around these elements significantly easier. For example, for a banking application, performance and resiliency are key dimensions. 
    * Relevant（相关）: These dimensions need to be considered at a feature level, but are unlikely to guide architecture choices. For example, code metrics around the quality of code base are important but not key. 
    * Not Relevant(不相关): Design and technology choices are not impacted by these types of dimensions. For example, process metrics such as cycle time (the amount of time to move from design to implementation, may be important in some ways but is irrelevant to architecture). As a result, fitness functions for it are not necessary.
* Review Fitness Functions A fitness function review is a meeting with key business and technical stakeholders with the goal of updating fitness functions to meet design goals.


## Chapter 3. Engineering Incremental Change
-------

* Our definition of evolutionary architecture implies incremental change, meaning the architecture should facilitate change in small increments. (意味着架构应该通过小的增量来促进变化)
* Combining Fitness Function Categories（结合恰当功能进行分类）. Fitness function categories often intersect when implementing them in mechanisms like deployment pipelines.
    1. atomic + triggered: This type of fitness function is exemplified by unit and functional tests run as part of software development.
    2. holistic + triggered: Holistic, triggered fitness functions are designed to run as part of integration testing via a deployment pipeline.
    3. atomic + continual: Continual tests run as part of the architecture, and developers design around their presence.
    4. holistic + continual: Holistic, continual fitness functions test multiple parts of the system all the time.

**PenultimateWidgets deployment pipeline**
![evolutionary-pipeline](http://oo6gt25nl.bkt.clouddn.com/evolutionary-pipeline.jpg)

PenultimateWidgets’ deployment pipeline consists
of six stages. 

* Stage 1 — Replicating CI The first stage replicates the behavior of the former CI server, running unit, and functional tests.
* Stage 2 — Containerize and Deploy Developers use the second stage to build containers for their service, allowing deeper levels of testing, including deploying the containers to a dynamically created test environment.
* Stage 3 — Atomic Fitness Functions In the third stage atomic fitness functions, including automated scalability tests and security penetration testing, are executed. This
* Stage 4 — Holistic Fitness Functions The fourth stage focuses on holistic fitness functions, including testing contracts to protect integration points and some further scalability tests.
* Stage 5a — Security Review (manual) This stage includes a manual stage by a specific security group within the organization to review, audit, and assess any security vulnerabilities in the codebase.
* Stage 5b — Auditing (manual) PenultimateWidgets is based in Springfield, where the state mandates specific auditing rules.
* Stage 6 — Deployment The last stage is deployment into the production environment.


## Chapter 4. Architectural Coupling
-------

* Discussions about architecture frequently boil down to coupling: how the pieces of the architecture connect and rely on one another（架构的各个部分是如何相互连接和依赖的）.
* In evolutionary architecture, architects deal with architectural quanta, the parts of a system held together by hard-to-break forces.
* The relationship between modules, components and quanta
![evolutionary-quantum](http://oo6gt25nl.bkt.clouddn.com/evolutionary-quantum.jpg)

* the outermost container is the quantum: the deployable unit that includes all the facilities required for the system to function properly, including data.
* Within the quantum, several components exist, each consisting of code (classes, packages, namespaces, functions, and so on). An external component (from an open source project) also exists as a library, a component packaged for reuse within a given platform. Of course, developers can mix and match all possible combinations of these common building blocks.

**架构风格演变**

* Big Ball of Mud
    1. Incremental change: 
    2. Guided change via fitness functions: 
    3. Appropriate coupling: 
* Monoliths
    1. Incremental change: 
    2. Guided change via fitness functions: 
    3. Appropriate coupling: 
* Layered architecture
    1. Incremental change: 
    2. Guided change via fitness functions: 
    3. Appropriate coupling: 
* Modular monoliths: A modular monolith contains logical grouping of functionality with well-defined isolation between modules.
    1. Incremental change: 
    2. Guided change via fitness functions: 
    3. Appropriate coupling: 
* Microkernel
    1. Incremental change: 
    2. Guided change via fitness functions: 
    3. Appropriate coupling: 
* Event-Driven Architectures: Event-driven architectures (EDA) usually integrate several disparate systems together using message queues. There are two common implementations of this type of architecture: the broker and mediator patterns.
    1. Incremental change: 
    2. Guided change via fitness functions: 
    3. Appropriate coupling: 
* Service-Oriented Architectures: ESB-driven SOA A particular manner of creating SOAs became popular several years ago, building an architecture based around services and coordination via a service bus — typically called an Enterprise Service Bus (ESB).
    1. Incremental change: 
    2. Guided change via fitness functions: 
    3. Appropriate coupling: 
* Microservices architectures partition across domain lines, embedding the technical architecture
    1. Incremental change: 
    2. Guided change via fitness functions: 
    3. Appropriate coupling: 
* Service-based architectures A more commonly used architectural style for migration is a service-based architecture, which is similar to but differs from microservices in three important ways: service granularity, database scope, and integration middleware.
    1. Incremental change: 
    2. Guided change via fitness functions: 
    3. Appropriate coupling: 
* “Serverless” Architectures: “Serverless” architectures are a recent shift in the software development equilibrium, with two broad meanings, both applicable to evolutionary architecture.
    1. Incremental change: 
    2. Guided change via fitness functions: 
    3. Appropriate coupling: 


## Chapter 5. Evolutionary Data
-------

## Chapter 6. Building Evolvable Architectures
-------

## Chapter 7. Evolutionary Architecture Pitfalls and Antipatterns
-------

## Chapter 8. Putting Evolutionary Architecture into Practice
-------

