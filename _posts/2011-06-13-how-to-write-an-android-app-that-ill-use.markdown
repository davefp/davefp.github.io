---
comments: true
date: 2011-06-13 10:00:46
layout: post
slug: how-to-write-an-android-app-that-ill-use
title: How To Write an Android App That I'll Use
wordpress_id: 573
categories:
- rants
tags:
- android
---

Last week, [Mark Jaquith](http://markjaquith.com/) posted an article entitled '[How to write a WordPress plugin that I’ll use](http://markjaquith.wordpress.com/2011/06/07/how-to-write-a-plugin-that-ill-use/)'. It occurred to me that a similar document about Android apps would be a useful and fun thing to write. A lot of his points carry over, but there are some Android-specific points that I’d like to expand upon.





## Use shared functionality, use shared data


One of my favourite things about Android is the ability to re-use functionality from other apps. Some of these are no-brainers: If you need the user to select a picture, use ACTION_PICK. Email? Use ACTION_SEND. This extends beyond the standard system intents. [OpenIntents](http://www.openintents.org) has a whole [slew of intents](http://www.openintents.org/en/intentstable) that you can use to accomplish any number of tasks. Before you write a given feature into your app, go there and see if someone has already done it. You'll save time, effort, and won't have to maintain the extra code. As a concrete example, Omnivore uses Zebra Crossing's barcode scanning intent to read barcodes.


## Share your functionality, share your data


My first point wouldn't be possible if developers didn't make their features available for reuse in the first place. You should do this. Take a look to see if your app performs a function that has an existing intent and if there isn't one, create it yourself. Omnivore doesn't do this yet, but eventually I want other apps to be able to access the food list for other purposes (e.g. creating shopping lists, finding recipes).


## Don't ask for unnecessary permissions


I was once in the market for a chess clock app. The most popular one at the time asked for both internet and location permissions. Sorry, what? How are either of those going to improve the application for me, the user. Advertising doesn't count as a valid reason for including a permission by the way. I'm fine with apps that make calls to ad-servers on the back of legitimate internet access, but including permissions for the sole purpose of showing ads isn't on.


## Use Android UI/UX conventions


When I press the menu button in an app and nothing happens, I get sad. Similarly, whenever I see an iPhone-esque back button in the top left I shake my head in disappointment. Users (myself included) expect certain things to function certain ways. Imagine installing a desktop app that didn't use the default keyboard shortcuts for copy/paste, undo/redo or save/load. Everything still works, but it feels out of place and wrong.
