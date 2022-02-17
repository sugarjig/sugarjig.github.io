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
