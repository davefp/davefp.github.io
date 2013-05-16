---
comments: true
date: 2011-11-21 10:00:28
layout: post
slug: how-to-examine-post-requests-using-postcatcher
title: How To Examine POST Requests Using PostCatcher
wordpress_id: 851
categories:
- tutorials
---

A little while back [I wrote about Localtunnel](http://theflyingdeveloper.com/testing-webhooks), a tool I use to test webhooks. Today I'd like to talk about [PostCatcher](http://postcatcher.in/), which is a superb service for examining POST requests easily.

[![](/a/2011-11-21-how-to-examine-post-requests-using-postcatcher/Screen-shot-2011-11-20-at-10.49.10-.png)](http://theflyingdeveloper.com/blog/wp-content/uploads/2011/11/Screen-shot-2011-11-20-at-10.49.10-.png)PostCatcher allows you to create a url that serves two purposes:



	
  1. When you POST to it, PostCatcher will record the request

	
  2. When you GET the page in a browser, it displays the previous requests for you to examine


Investigating an unfamiliar webhook is as easy as registering your PostCatcher url to receive updates and then generating some requests. The PostCatcher page will update in real-time with the new data, allowing you to see and examine out the structure easily.


## Alternatives


Before PostCatcher I used a similar service called [PostBin](http://www.postbin.org/). It's written by [progrium](https://github.com/progrium), who is also responsible for Localtunnel. Unfortunately I've found that PostBin is unavailable more often than not due to being over quota, hence the switch to PostCatcher. The latter also benefits from a slicker ui as well as login via. Github allowing you to keep track of your previously created 'catchers'.
