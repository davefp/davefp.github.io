---
comments: true
date: 2009-11-19 19:58:13
layout: post
slug: introducing-lighthouse-keeper
title: 'Introducing: Lighthouse Keeper'
wordpress_id: 179
categories:
- limelight
tags:
- adobe AIR
- api
- bug tracking
- entp
- flex
- lighthouse
- REST
---

**_Update: Oops! It looks like there is already a Lighthouse-related app called Lighthouse Keeper (it can be found [here](http://www.mcubedsw.com/software/lighthousekeeper)). Future versions of my app will have a different name to avoid confusion_**.

[Lighthouse](http://lighthouseapp.com/) is a lightweight hosted bug-tracker produced by [entp](http://entp.com/). It's advertised as "Beautifully Simple Issue Tracking".

Lighthouse Keeper is a small AIR app produced by me. It allows you to export Lighthouse ticket info to a csv file.


## Background


I learned about the existence of Lighthouse earlier this week at a job interview. I've used a couple of bug-trackers before, but Lighthouse had never appeared on my radar. The two guys interviewing me got into a spirited conversation with each other about the various pros and cons of different bug tracking solutions, but Lighthouse in particular. One of their comments was


> I wish there was a way to export my tickets as a csv file.

Maybe someone will come up with something using the API.


That's a challenge if I've ever heard one.


## Implementation


I'm a big fan of Flash/Flex, so I set to work that evening on a small app that would grab Lighthouse projects for a given domain then allow you to perform searches on the tickets in an individual project and export the results. Fortunately, flex is designed around tasks just like this.

Lighthouse has a fairly comprehensive REST API available for developers. Using either your regular login info or a generated API key, you have access to (as far as I can tell) all of the functionality you get from the regular web interface.

Thanks to this, retrieving data from the API was achieved in less than an hour, and then the rest of the time (about 6 more hours) was spent wrangling it and tweaking the interface.

The current incarnation is still very much an experimental product, but I want to release it to let people (and prospective employers) test it out and see what they think.


## Download


So, without further ado, here it is:

[Lighthouse Keeper version 0.1](http://theflyingdeveloper.com/misc/lighthousekeeper.air)

As always: Questions, comments, suggestions and criticism are welcomed and encouraged.
