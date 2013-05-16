---
comments: true
date: 2011-10-03 10:00:13
layout: post
slug: whats-next-for-life-dial
title: What's Next For Life Dial?
wordpress_id: 819
categories:
- news
tags:
- android
- life dial
- magic the gathering
---

If you're not familiar, [Life Dial](https://market.android.com/details?id=com.inflatableapps.lifedial) is my Magic: The Gathering life tracker app for android. I initially wrote it for personal use, but have since made it available on the Android Market for free.

I currently have just shy of 350 steady installs (which I'm really pleased with), but that seems to be where things have evened out. As the app is free I don't want to spend money on advertising it at the moment, so I figure that if I want more people to install it I should work on making it appeal to a wider audience. Here's how I plan on doing that.


## The Plan




### Multiple Players


Right now the app only supports a single player - The idea is that you use it to track your own life (and poison) total and let the other player track theirs. This works ok for casual play, but since then I've attended a couple of pre-release events that are a little more competitive and realized that you should always keep track of **all** players' life. Therefore the first big feature I want to add is the ability to track multiple life totals simultaneously.


### Poison and Other Counters


The app was written while Scars of Mirrodin was the current expansion. As a result, it gives equal billing to poison and life. Now that the game has moved on a little bit, infect decks are seen less often. At the same time I've had a lot of requests to track other totals: Mainly related to the Commander set Wizards released this summer. To kill as many birds as I can with a single stone, I'm going to be working on a setting that allows the user to display an arbitrary number of totals per player, which can be used for whatever they like. Life, poison, commander damage, or anything else you can think of.


### Monetization


Finally, I want to experiment with monetizing the app. The current version will always be free, but the new features I mentioned above (the '2.0' features) will probably be made available as an in-app purchase. This is as much an experiment in using the Google Checkout API as anything else, but I'm also interested to see whether people will pay a dollar for a life tracking app.


## Implementation Challenges


The big thing I have to figure out is how I'm going to display all this extra information on the screen. The current interface is designed to maximize the size of each of the elements so that they're as easy to use as possible. If I add more players/counters then I have to either make everything smaller or move some of the data off-screen and have the user page through it. Neither of those is particularly appealing to me so I'll definitely have to spend some time experimenting to see which one works better.

I'm hoping to work on all these changes over the next month, so with any luck Life Dial 2.0 will appear in the Market early November.
