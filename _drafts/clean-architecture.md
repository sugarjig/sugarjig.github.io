---
title: "Clean Architecture: A Book Review"
---

A book that I've wanted to read for a long time is [Clean Architecture](https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164/) by Robert C. Martin (a.k.a Uncle Bob). I came across his [2012 blog post](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) on the topic. It made so much sense to me that I went on to design an entire web service around its core principles. I evangalize it to this day. But I never read the book, published in 2018.

Having just finished it, I wanted to summarize the highlights here, as a way to solidify its concepts.

## Introduction

There is no difference between design and architecture. Low-level details and high-level details are part of the same whole.

> The goal of software architecture is to minimize the human resources required to build and maintain the required system.

If more and more effort is required to make changes to the software, then the design is bad. If the amount of effort stays low, then the design is good. Developers are under the false impression that sacrificing quality allows them to go faster in the short term. In reality, developers never switch back to cleanup mode. It is always faster to keep your code clean and tidy.

> The only way to go fast, is to go well.

Developers often think that they can rebuild from scratch, but better. But the same overconfidence that drove the first poor design will only create the same messes.

Programs that work perfectly but are too expensive to change will become useless once the requirements change. Programs that are easy to change will keep up with the requirements. Business managers may say that they want current functionality over future ease of change, but they will not be happy if the developers allow the system to become too expensive to change. Developers are hired to maintain a good architecture. It is their responsibility to assert its importance. It is always a struggle, but developers are stakeholders and therefore have an interest in the software's success.

## Programming Paradigms

Each paradigm takes capabilities away from the programmer.

### Structured Programming

> Structure programming imposes discipline on direct transfer of control.

Structured programming introduced functional decomposition, allowing programmers to break problems down into smaller and smaller problems.

### Object-Oriented Programming

> Object-oriented programming imposes discipline on indirect transfer of control.

Object-oriented programming introduced polymorphism, which enables dependency inversion. Components can be deployed and devoloped independently.

### Functional Programming

> Functional programming imposes discipline upon assignment.

Functional programming introduced immutable variables. This eliminates the problems faced in concurrent programming. It is best to move as much code as possible into immutable components.

## Design Principles

The goal of SOLID principles is to create software that

- tolerates change
- is easy to understand
- is the basis of components that can be used in many software systems

### Single Responsibility Principle (SRP)

> A module should be responsible to one, and only one, actor.

> Separate the code that different actors depend on.

### Open-Closed Principle (OCP)

Higher-level code should not be impacted by changes made to lower-level code. We want to make the easy to change without increasing the impact of change.

### Liskov Substitution Principle (LSP)

Components in a system must conform to a contract in order be substitutable for one another.

### Interface Segregation Principle (ISP)

Don't depend on things you don't use.

### Dependency Inversion Principle (DIP)

> Details should depend on policies.

## Component Principles

### Component Cohesion

#### Reuse/Release Equivalence Principle (REP)

> The granule of reuse is the granule of release.

The classes and modules in a component must be part of a cohesive group.

#### Common Closure Principle (CCP)

> Gather into components those classes that change for the same reasons and at the same times. Separate into different components those classes that change at different times and for different reasons.

This is similar to the Single Responsibility Principle.

> Gather together those things that change at the same times and for the same reasons. Separate those things that change at different times or for different reasons.

#### Common Reuse Princicple (CRP)

> Don't force users of a component to depend on things they don't need.

"Classes that are not tightly bound to each other should not be in the same component."

This is related to the Interface Segregation Principle.

> Don't depend on things you don't need.

#### Tension

- REP and CCP: Too many unneeded releases
- CCP and CRP: Hard to reuse
- CRP and REP: Too many components change

### Component Coupling

#### Acyclic Dependencies Principle

> Allow no cycles in the component dependency graph.

Break down the system into separately releaseable components. Each component can be owned by a single developer or an entire team. When a component is working, it is assigned a release version and made available to other teams.

#### The Stable Dependencies Principle

> Depend in the direction of stability

A component that his hard to change should not depend on volatile components.

Stability: when a component has many incoming dependencies

- Fan-in: Incoming dependencies
- Fan-out: Outgoing dependencies
- Instability (I): I = Fan-out / (Fan-in + Fan-out). Range of 0-1, where 0 is maximally stable, and 1 is maximally unstable

#### The Stable Abstractions Principle

> A component should be as abstract as it is stable.

> Dependencies run in the direction of abstraction.

- Nc: Number of classes in component
- Na: Number of abstract classes and interfaces in component
- Abstractness (A): A = Na / Nc. 0 means no abstrac classes, 1 means no concrete classes

- The Zone of Pain: highly stable and concrete; database schema is an example; nonvolatile components are harmless here
- The Zone of Uselessness: maximally abstract, no dependencies
- The Main Sequence: line between stable/abstract and unstable/concrete; best to put components on or close to this line

Distance (D): D = |A + I - 1|

One can perform statistical analysis of a design and calculate the mean and variance of D for all components.

Distance (D): D = |A + I - 1|

One can perform statistical analysis of a design and calculate the mean and variance of D for all components. A "good" design would have a mean and variance of close to zero.

## Architecture

### What Is Architecture?

Architecture is the shape of the system, the purpose of which is to facilitate development, deployment, operation, and maintenance.

> The strategy behind that facilitation is to leave as many options open as possible, for as long as possible.

"The ultimate goal is to minimaize the lifetime cost of the system and to maximize programmer productivity."

Architecture should allow software to be deployed in a single action. It should also make use cases, features, and required behaviors first-class citizens that are immediately visible to developers.

The options we need to leave open are the details that don't matter. All software can be broken down into policy and details. Policy is where the true value lies. Architecture makes details irrelevant to the policy, allowing decisions to be delayed and deferred. The longer you wait to make the decisons, the more information you have to make the properly.

> A good architect maximizes the number of decisons not made.

### Independence

Architects should leave operational decisions open. "A good architecture makes the system easy to change, in all the ways that it must change, by leavng options open."

"If two apparently duplicated sections of code evolve along different paths--if they change at different rates, and for different reasons--then they are not true duplicates."

"Resist the temptation to commit the sin of knee-jerk elimination of duplication." "Creating the separate view model is not a lot of effort, and it will help you keep the layers propertly decoupled."

Decoupling layers can be done at the following levels:
- Source level
- Deployment level: JARs, DLLs, shared libraries, etc.; communication over functions calls or IPC/sockets/shared memory
- Service level: communication over network

Service-level decoupling is expensive in development time and system resources

Good architecture can start as a monolith, then grow to a set of independently deployable units, up to independent services. It can also allow for reversing back to a monolith.

### Boundaries: Drawing Lines

Draw lines between things that matter and things that don't. Business rules don't need to anything about the database schema or query language. "The IO is irrelevant."

Treat the UI and database as plugins. Replacing one for another might not be trivial, but it could be practical if we start with the idea of them as plugins.

Draw boundaries on the axis of change. Components on one side change at different rates and for different reasons.

### Policy and Level

Level: The distance from the inputs and outputs.

The farther away from the inputs and outputs, the higher the level. Source code dependencies should be decoupled from data flow and coupled to level.

### Business Rules

Business rules make or save the business money. This is true whether executed on a computer or not.

_Critical Business Rules_ would exist even if they were not automated.

_Critical Business Data_ would exist even if the system were not automated.

__Entity:__ "an object within our computer system that embodies a small set of critical business rules operating on Critical Business Data." It is "pure business and nothing else."

__Use case:__ "a description of the way that an automated system is used." It describes _application-specific_ business rules as opposed to Critical Business Rules.

"Use cases control the dance of the Entities." They "do not descibe how the system appears to the user."

### Screaming Architecture

"software architectures are structures that support the use cases of the system."

"Frameworks are options to be left open."

"Frameworks are tools, not ways of life"

"Develop a strategy that prevents the fraemwork from taking over that architecture."

### The Clean Architecture

This looks the same as the blog post.

Several architectures have emerged over time.

<insert image>

- Hexagonal Architecture
- DCI
- BCE

All have the following characteristics:

- Independent of frameworks
- Testable
- Independent of the UI
- Independent of the database
- Independent of any external agency

> _Dependency Rule:_ Source code dependencies must point only inward, toward higher-level policies.

"The name of something declared in an outer circle must not be mentioned by the code in an inner circle."

- __Entities__: "encapsulate enterprise-wide Critical Business Rules"
- __Use Cases__: "contain application-specific business rules; orchestrate the flow of data to and from the entities, and direct those entities to use their Critical Business Rules to achieve the goals of the use case."
- __Interface Adapters__: "convert data from the format most convenient for the use cases and entities, to the format most convenient for some external agency such as the database or the web."
- __Frameworks and Drivders__: "frameworks and tools such as the database and the web framework." "where all the details go"

You may need more than four circles, but the Dependency Rule always applies.

Use the Dependency Inversion Principle so that the source code dependencies oppose the flow of control. When crossing boundaries, use the data in the form most convenient for the inner circle.

### Presenters and Humble Objects

__Humble Object pattern:__ "originally identified as a way to help unit testers to separate behaviors that are hard to test from hehaviors that are easy to test."

View: humble object that is hard to test
Presenter: testable object; formats data for the View, places it in a data structure called View Model

The View merely loads data form the View Model onto the screen. (making it humble)

ORMs should exist in the database layer; are another example of a Humble Object

The Humble Object is useful at architecture boundaries. They increase the testability of the system.

### Partial Boundaries

"Do all the work necessary to create independently compilable and deployable components, and then simply keep them together in the same component."

### Layers and Boundaries

Fully implemented boundaries are expensive. When they are ignored, they are very expensive to add in later. You must watch the system as it evolves, adding boundaries when required.

### The Main Component

"The Main component is the ultimate detail--the lowest-level policy." "The dirtiest of all the dirty components."

### Services: Great and Small

Using services is not necessarily an architecture. The services could still be strongly coupled by the data they share.

Large enterprise systems can still be monoliths. Services cannot always be independently developed, deployed, and operated.
