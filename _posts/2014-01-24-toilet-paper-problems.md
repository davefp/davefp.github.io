---
layout: post
title: "Toilet Paper Problems"
date: 2014-01-24 09:38
comments: true
categories:
---

There's a class of problem I like to call 'Toilet Paper' problems. A toilet paper problem is one whose effort required to resolve is proportional to its current urgency. The more urgent the issue, the harder it is to resolve.

The canonical example is as follows: In your home, you have (I assume) a quantity of toilet paper. While there's plenty of toilet paper there's no problem. It's also easy to go out and buy more, but there's little incentive to because you already have some.

The moment you run out though, you have a huge problem. Not only are you out of toilet paper, but by the time you notice you're definitely not in a position to go and buy more (or go to the closet down the hall, depending on your individual toilet paper logistics strategy).

I see toilet paper problems in software all the time. Testing is an area fraught with TPPs. You think can get away without writing tests for simple software because you can hold the whole thing in your head at once. It's also trivial to write tests for such software, so I'll do it later, right?

But then there's a terrible moment, as the skin touches the porcelain, when you realize your software has reached a critical complexity level. You can no longer write new features without the risk of breaking existing components.

If only you'd had tests! Writing them earlier would have been trivial, but now you're stuck with broken software and no alternative but to call for help.

Know this: Small issues that are easy to fix now have a habit of suddenly becoming huge problems. Don't skimp on things like tests, staging environments, or proper software architecture because one day you'll run out of toilet paper.