---
comments: true
date: 2010-02-18 21:01:21
layout: post
slug: complicated-vs-simple
title: Complicated vs. Simple
wordpress_id: 224
categories:
- rants
tags:
- excel
- lesson
- matplotlib
- python
---

As a software developer, I'm used to being given tough problems and I'm not shy about building custom solutions from scratch to solve them. However, this is not always necessary. I recently learned that taking a step back and considering simpler solutions is always worthwhile.

This week, I was tasked with gathering and analyzing some data at my job. The task involved generating a couple of thousand data-points and examining their spread. The first part was easy, and accomplished in a couple of hours. What happened next was interesting though.

My script to gather/generate the data was written in python, so by the end of each run I had a large data structure in memory with all the results in. After printing some random samples to make sure the data was all in the correct ranges, I considered how to analyze it. A graph seemed the obvious choice and for my particular data-set an x/y scatter plot would be very useful. This decided, I went looking for a way of drawing such a thing.

My thoughts immediately went to finding a python library that would render the chart for me. After asking around a couple of my fellow developers I took a look at [matplotlib](http://matplotlib.sourceforge.net/) and was very impressed. It seemed to offer a huge range of different graph and chart types, along with a whole host of rendering options. I say 'seemed', because I never actually used it. I realized that there was a much simpler solution to my problem.

Excel!

Excel, the spreadsheet software I was taught to use way back when I was 13 and not touched since. Technically, it's all the things I dislike about software: bloated, over-engineered and closed-source. However, it does draw some pretty good charts with minimal work. Five minutes after realizing this, I had a thoroughly understandable representation of my data without writing a single extra line of code. Who'd have thought?

Could matplotlib have made the same chart? Of course. It probably would have had more colours, been in 3D and made you tea when you got up in the morning. On the downside, I didn't really need any of those things, and it would have taken a whole afternoon, if not longer.

Similarly, I could have used an open-source solution such as OpenOffice or something lightweight like Google Docs. Excel just happened to be the most convenient option as it was already installed on my laptop at work.

The lesson I drew from this is that even though I _can _do some pretty cool things, I don't always _have _to. Sometimes, existing solutions perform a task well enough and much faster to boot.
