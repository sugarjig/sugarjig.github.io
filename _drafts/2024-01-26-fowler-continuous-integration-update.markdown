---
title: "Continuous Integration: Martin Fowler's Big 2024 Update"
date: 2024-01-26
---

Martin Fowler recently updated his [Continuous Integration](https://martinfowler.com/articles/continuousIntegration.html) article. It's a great read, and I highly recommend it. I've been doing continuous integration for a long time, and I learned a lot from it.

recently shared with other engineers - misunderstanding of CI - the point is to integrate as frequently as possible
devs are overly concerned with continuing progress but it's only progress for them (https://graphite.dev/)
they don't focus on merging their changes and ensuring that the team as a whole is making progress

devs come up with solutions like "stacked PRs" - misses the point
the problem is a social one, not a technical or logistical one
if you're not getting your changes into mainline, you're not communicating with your team
"how do my changes affect your work?"
when you practice real CI, you are giving everyone a chance to see if your changes integrate well with theirs

> Semi-Integration (it isn't Continuous Integration)
>
> There's a common confusion about the term “integration”. Many people who work on branches, perhaps a feature branch or a personal development branch, will regularly pull changes from mainline. They check that these don't break their work by building and testing after the pull. They may use a CI Service to do this, checking that any change to mainline doesn't break their branch.
>
> But this isn't the full integration process. Full mainline integration requires that developers push their work back into the mainline. If they don't do that, then other team members can't see their work and check for any conflicts. This semi-integration doesn't prevent branches from diverging, conflicts from festering, and all the problems of low-frequency integration. Continuous Integration requires full mainline integration, no code sits on a branch longer than a few hours without being pushed into the mainline.

CI is all about communication. I'm not sure even Martin Fowler emphasizes this enough.

From "Everyone Pushes Commits to the Mainline Every Day"

> Integration is primarily about communication. Integration allows developers to tell other developers about the changes they have made. Frequent communication allows people to know quickly as changes develop.

To put it a different way, if you are not merging your changes into main frequently, you are not communicating with your team.

From "Everyone can see what's happening"

> Continuous Integration is all about communication, so we want to ensure that everyone can easily see the state of the system and the changes that have been made to it.

> One of the most important things to communicate is the state of the mainline build.

From "Less time wasted in integration"

> What's happening here is that we are seeing much closer collaboration between the members of the team. Should two developers make decisions that conflict, we find out when we integrate. So the less time between integrations, the less time before we detect the conflict, and we can deal with the conflict before it grows too big. **With high-frequency integration, our source control system becomes a communication channel, one that can communicate things that can otherwise be unsaid.**

======

many concerns about including "dead" code in production
use my favorite techniques
- [Feature Flags](https://martinfowler.com/bliki/FeatureFlag.html)
- [Branch By Abstraction](https://martinfowler.com/bliki/BranchByAbstraction.html)
- [Parallel Change](https://martinfowler.com/bliki/ParallelChange.html)

long, but well worth your time. faster than reading the book
Note: this advice applies to professional software teams working closley together every day. I can't speak to the best way to work on open source projects.
Added to my Resources page

CI serves as a critical foundation to Continuous Delivery

frequently merging changes reduces the risk of integration problems
holding on to feature branches for a long time increases the risk of integration problems

![Your Communication's Bad](../assets/images/zoidberg.jpg)