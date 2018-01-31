---
layout: post
title:  "《Building Evolutionary Architectures》读书笔记"
subtitle: "构建进化软件架构 读书笔记"
date:   2018-01-30 18:00:00
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
![evolutionary-scope](http://oo6gt25nl.bkt.clouddn.com/evolutionary-scope.jpg){:height="100%" width="100%"}

* An evolutionary architecture consists of three primary aspects: **incremental change, fitness functions, and appropriate coupling**.

## Chapter 2. Fitness Functions
-------

* The word guided（进化软件架构定义中的指导性） indicates that some objective exists that architecture should move toward or exhibit. 

* We use this concept to **define architectural fitness functions**: 
    * An architectural fitness function provides an objective integrity assessment of some architectural characteristic(s). 
    * 恰当功能提供了一些架构特征的客观完整性评估
* The systemwide fitness function is crucial for an architecture to be evolutionary, as we need some basis to allow architects to compare and evaluate architectural characteristics against one another.
![evolutionary-systemwide](http://oo6gt25nl.bkt.clouddn.com/evolutionary-systemwide.jpg){:height="100%" width="100%"}

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
![evolutionary-pipeline](http://oo6gt25nl.bkt.clouddn.com/evolutionary-pipeline.jpg){:height="100%" width="100%"}

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
![evolutionary-quantum](http://oo6gt25nl.bkt.clouddn.com/evolutionary-quantum.jpg){:height="100%" width="100%"}

* the outermost container is the quantum: the deployable unit that includes all the facilities required for the system to function properly, including data.
* Within the quantum, several components exist, each consisting of code (classes, packages, namespaces, functions, and so on). An external component (from an open source project) also exists as a library, a component packaged for reuse within a given platform. Of course, developers can mix and match all possible combinations of these common building blocks.

**架构风格演变**

分别从Incremental change（增量更改）、Guided change via fitness functions（恰当功能指导变更）和Appropriate coupling（适当耦合）三个方面，分析历史过程中的各个架构风格

#### Big Ball of Mud

1. 增量更改: Making any change in this architecture is difficult.
2. 指导变更: Building fitness functions for this architecture is difficult because no clearly defined partitioning exists.
3. 适当耦合: This architectural style is a good example of inappropriate coupling.

#### Monoliths

1. 增量更改: Large quantum size hinders incremental change because high coupling requires deploying large chunks of the application.
2. 指导变更: Building fitness functions for monoliths is difficult but not impossible.
3. 适当耦合: A monolithic architecture, with little internal structure outside simple classes, exhibits coupling almost as bad as a Big Ball of Mud.

#### Layered architecture
Each layer represents a technical capability, allowing developers to swap out technical architecture functionality easily.

![evolutionary-layer](http://oo6gt25nl.bkt.clouddn.com/evolutionary-layer.jpg){:height="100%" width="100%"}

1. 增量更改: Developers find it easy to make some changes in this architecture, particularly if those changes are isolated to existing layers.
2. 指导变更: Developers find it easier to write fitness functions in a more structured version of a monolith because the structure of the architecture is more apparent.
3. 适当耦合: One of the virtues of monolith architectures is easy understandability. Layered architectures allow for easy evolution of the technical architecture partitions defined by the layers.

#### Modular monoliths

A modular monolith contains logical grouping of functionality with well-defined isolation between modules.

![evolutionary-modular](http://oo6gt25nl.bkt.clouddn.com/evolutionary-modular.jpg){:height="100%" width="100%"}

1. 增量更改: Incremental change is easy in this type of architecture because developers can enforce modularity. 
2. 指导变更: Tests, metrics, and other fitness function mechanisms are easier to design and implement in this architecture because of good separation of components, allowing easier mocking and other testing techniques that rely on isolation layers.
3. 适当耦合: A well-designed modular monolith is a good example of appropriate coupling. 

#### Microkernel

Consider another popular monolithic architectural style, the microkernal architecture, commonly found in browsers and integrated development environments (IDEs)

1. 增量更改: Once the core system is complete, most behavior should come from plug-ins, small units of deployment.
2. 指导变更: Fitness functions are typically easy to create in this architecture because of the isolation between the core and plug-ins.
3. 适当耦合: The coupling characteristics in this architecture are well defined by the microkernel pattern.

#### Event-Driven Architectures

Event-driven architectures (EDA) usually integrate several disparate systems together using message queues. There are two common implementations of this type of architecture: the broker and mediator patterns.

**Brokers**

In a broker EDA, the architectural components consist of the following elements: 

* message queues : Message queues implemented via a wide variety of technologies such as JMS (Java Messaging Service). 
* initiating event : The event that starts the business process. 
* intra-process events : Events passed between event processors to fulfill a business process. 
* event processors : The active architecture components, which perform actual business processing. When two processors need to coordinate, they pass messages via queues.

![evolutionary-broker](http://oo6gt25nl.bkt.clouddn.com/evolutionary-broker.jpg){:height="100%" width="100%"}

1. 增量更改: Broker EDAs allow incremental change in multiple forms. Building deployment pipelines for broker EDAs can be challenging because the essence of the architecture is asynchronous communication, which is notoriously difficult to test.
2. 指导变更: Atomic fitness functions should be easy for developers to write in this architecture because the individual behaviors of event processors is simple.
3. 适当耦合: Broker EDAs exhibit a low degree of coupling, enhancing the ability to make evolutionary change.

**Mediators**

The other common EDA pattern is the mediator, where an additional component appears: a hub that acts as a coordinator

![evolutionary-mediator](http://oo6gt25nl.bkt.clouddn.com/evolutionary-mediator.jpg){:height="100%" width="100%"}

1. 增量更改: Similar to broker EDAs, the services in a mediator EDA are typically small and self-contained. Thus, this architecture shares many of the operational advantages of the broker version.
2. 指导变更: Developers find it easier to build fitness functions for the mediator than for the broker EDA. The tests for individual event processors don’t differ much from the broker version. However, holistic fitness functions are easier to build because developers can rely on the mediator to handle coordination.
3. 适当耦合:  While many testing scenarios become easier with mediators, coupling increases, harming evolution.

#### Service-Oriented Architectures

ESB-driven SOA A particular manner of creating SOAs became popular several years ago, building an architecture based around services and coordination via a service bus — typically called an Enterprise Service Bus (ESB).

1. 增量更改: While having a well-established technical service taxonomy allows for reuse and segregation of resources, it greatly hampers making the most common types of change to business domains.
2. 指导变更: Testing in general is difficult within ESB-driven SOA.
3. 适当耦合: From a potential enterprise reuse standpoint, extravagant taxonomy makes sense.



#### Microservices

* Microservices architectures partition across domain lines, embedding the technical architecture

1. 增量更改: Both aspects of incremental change are easy in microservices architectures. 
2. 指导变更: Developers can easily build both atomic and holistic fitness functions for microservices architectures.
3. 适当耦合: Microservices architectures typically have two kinds of coupling: integration and service template.


#### Serverless

* “Serverless” Architectures: “Serverless” architectures are a recent shift in the software development equilibrium, with two broad meanings, both applicable to evolutionary architecture.

![evolutionary-baas](http://oo6gt25nl.bkt.clouddn.com/evolutionary-baas.jpg){:height="100%" width="100%"}


1. 增量更改: Incremental change in serverless architectures should consist of redeploying code — all the infrastructure concerns exist behind the abstraction of “serverless.” 
2. 指导变更: Fitness functions are critical in this type of architecture to ensure integration points stay consistent.
3. 适当耦合: From an evolutionary architecture standpoint, FaaS is attractive because it eliminates several different dimensions from consideration: technical architecture, operational concerns, and security issues, among others.


## Chapter 5. Evolutionary Data
-------

When we refer to the DBA, we mean anyone who designs the data structures, writes code to access the data and use the data in an application, writes code that executes in the database, maintains and performance tunes the databases, and ensures proper backup and recovery procedures in the event of disaster. DBAs and developers are often the core builders of applications, and should coordinate closely.

Option 1: No integration points, no legacy data


```
ALTER TABLE customer ADD firstname VARCHAR2( 60); 
ALTER TABLE customer ADD lastname VARCHAR2( 60); 
ALTER TABLE customer DROP COLUMN name;
```


Option 2: Legacy data, but no integration points

```
ALTER TABLE Customer ADD firstname VARCHAR2( 60); 
ALTER TABLE Customer ADD lastname VARCHAR2( 60); 
UPDATE Customer set firstname = extractfirstname (name); UPDATE Customer set lastname = extractlastname (name); 
ALTER TABLE customer DROP COLUMN name;
```

Option 3: Existing data and integration points

```
ALTER TABLE Customer ADD firstname VARCHAR2( 60); 
ALTER TABLE Customer ADD lastname VARCHAR2( 60); 

UPDATE Customer set firstname = extractfirstname (name); UPDATE Customer set lastname = extractlastname (name); 

CREATE OR REPLACE TRIGGER SynchronizeName
BEFORE INSERT OR UPDATE 
ON Customer 
REFERENCING OLD AS OLD NEW AS NEW 
FOR EACH ROW 
BEGIN 
    IF :NEW.Name IS NULL THEN :NEW.Name := :NEW.firstname | |' '| |: NEW.lastname; 
    END IF; IF :NEW.name IS NOT NULL THEN 
        :NEW.firstname := extractfirstname(: NEW.name); 
firstnam:NEW.lastname := extractlastname(: NEW.name); 
  END IF; 
END;

```


## Chapter 6. Building Evolvable Architectures
-------

### Mechanics 

Architects can operationalize these techniques for building an evolutionary architecture in three steps:

1. Identify Dimensions Affected by Evolution：First, architects must identify which dimensions of the architecture they want to protect as it evolves.
2. Define Fitness Function(s) for Each Dimension：A single dimension often contains numerous fitness functions. Then, for each dimension, they decide what parts may exhibit undesirable behavior when evolving, eventually defining fitness functions.
3. Use Deployment Pipelines to Automate Fitness Functions：Lastly, architects must encourage incremental change on the project, defining stages in a deployment pipeline to apply fitness functions and managing deployment practices like machine provisioning, testing, and other DevOps concerns.

### Retrofitting Existing Architectures 

Adding evolvability to existing architectures depends on three factors: component coupling, engineering practice maturity, and developer ease in crafting fitness functions.

For the first step in migrating architecture, developers identify new service boundaries. Teams may decide to break monoliths into services via a variety of partitioning as follows:

* Business functionality groups：A business may have clear partitions that mirror IT capabilities directly.

* Transactional boundaries：Many businesses have extensive transaction boundaries they must adhere to.

* Deployment goals：Incremental change allows developers to selectively release code on different schedules.



## Chapter 7. Evolutionary Architecture Pitfalls and Antipatterns
-------

* Antipattern: Vendor King：Rather than fall victim to the vendor king antipattern, treat vendor products as just another integration point. Developers can insulate vendor tool changes from impacting their architecture by building anticorruption layers between integration points.

* A typical software stack in 2016, with lots of moving parts

![evolutionary-2016](http://oo6gt25nl.bkt.clouddn.com/evolutionary-2016.jpg){:height="100%" width="100%"}


* Microservices eschew code reuse, adopting the philosophy of prefer duplication to coupling: reuse implies coupling, and microservices architectures are extremely decoupled.

* We’re not suggesting teams avoid building reusable assets, but rather evaluate them continually to ensure they still deliver value.


## Chapter 8. Putting Evolutionary Architecture into Practice
-------

**This includes both technical and business concerns, including organization and team impacts.**

### Organizational Factors 

The impact of software architecture has a surprisingly wide breadth on a variety of factors not normally associated with software, including team impacts, budgeting, and a host of others.

**Cross-Functional Teams**

these roles must change to accommodate this new structure, which includes the following roles:

1. Business Analysts：Must coordinate the goals of this service with other services, including other service teams. 
2. Architecture: Design architecture to eliminate inappropriate coupling that complicates incremental change.
3. Testing: Testers must become accustomed to the challenges of integration testing across domains,
4. Operations: Slicing up services and deploying them separately (often alongside existing services and deployed continuously) is a daunting challenge for many organizations with traditional IT structures.
5. Data: Database administrators must deal with new granularity, transaction, and system of record issues.




