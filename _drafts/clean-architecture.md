---
title: "Clean Architecture: A Book Summary"
toc: true
---

A book that I've wanted to read for a long time is [Clean Architecture](https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164/) by Robert C. Martin (a.k.a Uncle Bob). I came across his [2012 blog post](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) a while back. It made so much sense to me that I went on to design an entire web service around its core principles. I evangalize it to this day.

Despite hanging my hat on the blog post, I never read the book until now. I wanted to summarize the highlights here, as a way to solidify its concepts in my mind. This post is mainly for my own future reference.

-----

## Introduction

> The goal of software architecture is to minimize the human resources required to build and maintain the required system.

If more and more effort is required to make changes to the software, then the design is bad. If the amount of effort stays low, then the design is good. Developers are under the false impression that sacrificing quality allows them to go faster in the short term. In reality, developers never switch back to cleanup mode. It is always faster to keep your code clean and tidy.

> The only way to go fast, is to go well.

Developers often think that they can rebuild from scratch, but better. But the same overconfidence that drove the first poor design will only create the same messes.

Programs that work perfectly but are too expensive to change will become useless once the requirements change. Programs that are easy to change can keep up with the requirements.

Business managers may say that they want current functionality over future ease of change, but they will not be happy if the developers allow the system to become too expensive to change. Developers are hired to maintain a good architecture. It is their responsibility to assert its importance. It is always a struggle, but developers are stakeholders and therefore have an interest in the software's success.

## Programming Paradigms

A few paradigms have been introduced throughout the history of programming. Each paradigm takes capabilities away from the programmer.

### Structured Programming

> Structure programming imposes discipline on direct transfer of control.

Structured programming introduced functional decomposition, allowing programmers to break problems down into smaller and smaller problems. It removes the ability to use `goto` statements.

### Object-Oriented Programming

> Object-oriented programming imposes discipline on indirect transfer of control.

Object-oriented programming introduced polymorphism, which enables dependency inversion. Components can be deployed and devoloped independently.

### Functional Programming

> Functional programming imposes discipline upon assignment.

Functional programming introduced immutable variables. This eliminates the problems faced in concurrent programming. It is best to move as much code as possible into immutable components.

## Design Principles

The goal of [SOLID principles](https://en.wikipedia.org/wiki/SOLID) is to create software that

- tolerates change
- is easy to understand
- is the basis of components that can be used in many software systems

Many software engineers have heard of the SOLID principles, but in my experience they are often misunderstood and ignored as a result.

### Single Responsibility Principle (SRP)

> A module should be responsible to one, and only one, actor.

Put another way, if one actor requires a change to a module, that change should not affect other actors.

> Separate the code that different actors depend on.

### Open-Closed Principle (OCP)

> A software artifact should be open for extension but closed for modification.

Of all the SOLID principles, this is the one I had a hard time grasping. How can software be closed for modification? The key to understanding it is realizing that higher-level code should not be impacted by changes made to lower-level code. We want to make the code easy to change without increasing the impact of change. We can accomplish this by adhering to the other principles.

### Liskov Substitution Principle (LSP)

> If for each object o1 of type S there is an object o2 of type T such that for all programs P defined in terms of T, the behavior of P is unchanged when o1 is substituted for o2 then S is a subtype of T.

Cutting though the formal math definition, what this is saying is that components in a system must conform to a contract in order be substitutable for one another.

### Interface Segregation Principle (ISP)

This can be summed up as: Don't depend on things you don't use. If you avoid unnecessary dependencies, you avoid being affected by change.

### Dependency Inversion Principle (DIP)

> Details should depend on policies.

The rest of the book covers what details and policies are, but the important point is that policies should never depend on details, nor should they have any _knowledge_ of details.

## Component Principles

The component principles are like the SOLID principles, but at a larger scale. They tell us how to build software systems out of smaller components.

### Component Cohesion

These principles guide how to decide which classes should be included in which components.

#### Reuse/Release Equivalence Principle (REP)

> The granule of reuse is the granule of release.

The classes and modules in a component must be part of a cohesive group. They should be releasable together.

#### Common Closure Principle (CCP)

> Gather into components those classes that change for the same reasons and at the same times. Separate into different components those classes that change at different times and for different reasons.

This is similar to the Single Responsibility Principle. Both can be summed up as the following:

> Gather together those things that change at the same times and for the same reasons. Separate those things that change at different times or for different reasons.

#### Common Reuse Princicple (CRP)

> Don't force users of a component to depend on things they don't need.

If you include unrelated code in your component, and that code changes, you will force users to recompile and revlidate their own code.

This is related to the Interface Segregation Principle. Both can be summed up as the following:

> Don't depend on things you don't need.

#### Tension

There is a tension among the three principles, which forms a triangle. If you focus too much on two of the three principles, you get the following:

- REP and CCP: Too many unneeded releases
- CCP and CRP: Hard to reuse
- CRP and REP: Too many components change

Architects must find a position in the triangle to meet the system's current needs, but also realize that those needs will change over time.

### Component Coupling

These principles deal with the relationships between components.

Martin introduces some very useful metrics in this part of the book.

#### Acyclic Dependencies Principle

> Allow no cycles in the component dependency graph.

Break down the system into separately releaseable components. Each component can be owned by a single developer or an entire team. When a component is working, it is assigned a release version and made available to other teams.

#### The Stable Dependencies Principle

> Depend in the direction of stability

A component that his hard to change should not depend on volatile components. An example would be a component containing business rules depending on the GUI component.

A component with many incoming dependencies is considered stable. It takes a lot of work to change this component.

##### Stability Metrics

- `Fan-in`: Incoming dependencies
- `Fan-out`: Outgoing dependencies
- `I: Instability`: `I = Fan-out / (Fan-in + Fan-out)`

Maximally stable components will have an `I` of 0, while maximally unstable components will have an `I` of 1.

#### The Stable Abstractions Principle

> A component should be as abstract as it is stable.

A stable component should be composed of interfaces and abstract classes so that it can be extended.

> Dependencies run in the direction of abstraction.

Put another way, concrete components depend on abstract components.

##### Abstraction Metrics

- `Nc`: Number of classes in component
- `Na`: Number of abstract classes and interfaces in component
- `A: Abstractness`: `A = Na / Nc`

An `A` of 0 means no abstract classes, while an `A` of 1 means nothing but abstract classes.

##### Plotting Stability and Abstraction

We can plot components on a graph with `I` as the x-axis and `A` as the y-axis.

- The Zone of Pain: area around `(0,0)`, highly stable and concrete; database schema is an example; nonvolatile components are harmless here
- The Zone of Uselessness: area around `(1,1)`, maximally abstract, no dependencies
- The Main Sequence: line between stable/abstract and unstable/concrete; best to put components on or close to this line

We can measure the distance from the Main Sequence of each component:

`D: Distance`: `D = |A + I - 1|`

One can perform statistical analysis of a design and calculate the mean and variance of D for all components. A "good" design will have a mean and variance of close to zero.

## Architecture

Now we finally put all these principles together and discuss how to make apply them to software architecture. This is the real meat of the Clean Architecture Book.

### What Is Architecture?

Architecture is the shape of the system, the purpose of which is to facilitate development, deployment, operation, and maintenance.

> The strategy behind that facilitation is to leave as many options open as possible, for as long as possible.

The ultimate goal of a good architecture is to minimaize cost and to maximize productivity.

Architecture should allow software to be deployed in a single action. It should also make use cases, features, and required behaviors first-class citizens that are immediately visible to developers.

Leave as many options open as possible. The options we need to leave open are the details that don't matter. Policy is the important part of the system. Architecture makes details irrelevant to the policy, allowing decisions to be delayed and deferred. The longer you wait to make the decisons, the more information you have to make them properly.

> A good architect maximizes the number of decisons not made.

### Independence

Architects should leave operational decisions open, which will make the system easy to change. 

Code that appears to be duplicated, but that changes at different rates and for different reasons, is not truly duplicated. An example is an entity vs. the related request/response models. This code should remain independent. It will take more effort up front, but will save effort in the long run.

Decoupling layers can be done at the following levels:
- Source
- Deployment (JARs, DLLs, shared libraries, etc.; communication over functions calls or IPC/sockets/shared memory)
- Service (communication over network)

Service-level decoupling is expensive in terms of development time and system resources. Good architecture can start as a monolith, then grow to a set of independently deployable units, up to independent services. It can also allow for reversing back to a monolith.

### Boundaries: Drawing Lines

Draw lines between things that matter and things that don't. Business rules don't need to anything about the database schema or query language. "The IO is irrelevant."

Treat the UI and database as plugins. Replacing one for another might not be trivial, but it could be practical if we start with the idea of them as plugins.

Draw boundaries on the axis of change. Components on one side change at different rates and for different reasons.

### Policy and Level

Level is defined as the distance from the inputs and outputs. The farther away from the inputs and outputs, the higher the level. Source code dependencies should be decoupled from data flow and coupled to level.

### Business Rules

Business rules make or save the business money. This is true whether executed on a computer or not. _Critical Business Rules_ would exist even if they were not automated. _Critical Business Data_ would exist even if the system were not automated.

__Entity:__ "an object within our computer system that embodies a small set of critical business rules operating on Critical Business Data." It is nothing but pure business.

__Use case:__ "a description of the way that an automated system is used." It describes _application-specific_ business rules as opposed to Critical Business Rules. It controls the "dance of the Entities." It does not define how the system is presented.

### The Clean Architecture

This entire chapter is essentially the same as Martin's original [blog post](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)

![The Clean Architecture](/assets/images/CleanArchitecture.jpeg)

Several architectures have emerged over time.

- [Hexagonal Architecture](https://en.wikipedia.org/wiki/Hexagonal_architecture_(software))
- [Data, Context, Interaction (DCI)](https://en.wikipedia.org/wiki/Data,_context_and_interaction)
- [Boundary-Control-Entity (BCE)](https://en.wikipedia.org/wiki/Entity-control-boundary)

All have the following characteristics:

- Independent of frameworks
- Testable
- Independent of the UI
- Independent of the database
- Independent of any external agency

The most important rule of the architecture is the _Dependency Rule_:

> Source code dependencies must point only inward, toward higher-level policies.

Code in an inner circle must not reference by name code in an outer circle

- __Entities__: "encapsulate enterprise-wide Critical Business Rules"
- __Use Cases__: "contain application-specific business rules; orchestrate the flow of data to and from the entities, and direct those entities to use their Critical Business Rules to achieve the goals of the use case."
- __Interface Adapters__: "convert data from the format most convenient for the use cases and entities, to the format most convenient for some external agency such as the database or the web."
- __Frameworks and Drivers__: "frameworks and tools such as the database and the web framework." This circle is "where all the details go."

You may need more than four circles, but the Dependency Rule always applies.

Use the Dependency Inversion Principle so that the source code dependencies oppose the flow of control. When crossing boundaries, use the data in the form most convenient for the inner circle.

### Presenters and Humble Objects

__Humble Object pattern:__ "originally identified as a way to help unit testers to separate behaviors that are hard to test from hehaviors that are easy to test."
__View:__ humble object that is hard to test
__Presenter:__ testable object; formats data for the View, places it in a data structure called View Model

The View merely loads data form the View Model onto the screen, making it humble. Another example of a Humble Object is an ORM, which should only exist in the database layer. The Humble Object is useful at architecture boundaries. They increase the testability of the system.

### Layers and Boundaries

Fully implemented boundaries are expensive. When they are ignored, they are very expensive to add in later. You must watch the system as it evolves, adding boundaries when required.

### The Main Component

That Main component is the "ultimate detail." It is the "dirtiest of all the dirty components."

### Services: Great and Small

Using services is not necessarily an architecture. The services could still be strongly coupled by the data they share.

Large enterprise systems can still be monoliths. Services cannot always be independently developed, deployed, and operated.

### The Test Boundary

Tests are part of the system. They participate in the architecture. Tests are the outermost circle in the architecture, and they are independently deployable.

Fragile tests can make the system rigid. To solve for this, design for testability. Business rules should be able to be tested without the GUI. Create a testing API that has superpowers that allow tests to bypass normal restrictions.

### Clean Embedded Architecture

Firmware isn't firm because of where it is stored, but because of how hard it is to change as the hardware changes.

_App-titude test_: "Programmers who just concern themselves with getting their app to work are doing their products and employers a disservice. There is much more to programming than just getting an app to work."

Make the embedded software testable outside of the target hardware. Use a hardware abstraction layer. It should be designed to fit the software's needs. Similarly, use an operating system abstraction layer.

## Details

### The Database Is a Detail

The database is not the data model. Inner circles should have no knowledge of the database schema. Only the outer most circles should have knowledge of the database.

### The Web Is a Detail

The GUI is a detail, and the WEB is an IO device.

### Frameworks Are Details

You must do everything the framework wants, and the framework authors owe you nothing. Frameworks tend to not follow a clean architecture. They violate the Dependency Rule. They want you to inerhit their code into your Entities, which are in the innermost circle. "The wedding ring is on your finger; and it's going to stay there."

> Don't marry the framework!

Treat the framework as a detail. Keep it in inthe outermost circles. For example, if using Spring, don't sprinkle `@autowired` throughout your business objects.

-----

Overall, Clean Architecture was a good read, but not an essential one. The book takes the ideas from the original blog post and expands on them, strengthening the case for using them. But the blog post alone is powerful enough that you can design an entire system around its concepts, like I did!
