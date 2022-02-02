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
