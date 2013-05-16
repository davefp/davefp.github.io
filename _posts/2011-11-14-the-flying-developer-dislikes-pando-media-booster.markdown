---
comments: true
date: 2011-11-14 10:00:43
layout: post
slug: the-flying-developer-dislikes-pando-media-booster
title: The Flying Developer Dislikes Pando Media Booster
wordpress_id: 834
categories:
- rants
tags:
- apb reloaded
- lol
- malware
- pando
---

![](/a/2011-11-14-the-flying-developer-dislikes-pando-media-booster/pando-logo.png)

I've been playing [League of Legends](http://leagueoflegends.com) over the last month or so, and I rather like it. But that's not what this post is about.

A couple of days after I installed the game, I noticed something strange. My network monitor gadget was showing an ongoing upload of about 1Mbit. Whaa? I don't remember telling anything to upload large amounts of data. Time to investigate!

Throwing open performance monitor, I saw a process called pmb.exe. Perfmon has a handy context menu feature that allows you to search the net for a process name, so I did that and found that the process was a program called [Pando Media Booster](http://www.pandonetworks.com/media-booster).


### This looks familiar...


Seeing that name jogged my memory. I'd seen it before, pulling the same trick. That time I had been trying [APB: Reloaded](http://apbreloaded.gamersfirst.com/). I decided to hit up the Riot Games forums to see if pmb.exe was linked to LoL in any way. Turned out it was.

It seems that the trend amongst many free-to-play games (including LoL) is to supplement their direct download sources with peer-to-peer services for distributing the game client. This makes a lot of sense from a cost-saving perspective, and I'm all for it. Pando Media Booster is a tool that facilitates this.


### Malware at Best


The trouble is, Pando seems consistently shady in the way it is installed and configured.  It's usually auto-installed along with whatever game you're installing. By default it's set to run on start up and has no cap on the upload speed it uses. It doesn't have a task tray icon so unless you're paying attention you'll have no idea it's running. That sounds a lot like malware to me.

Fortunately uninstalling Pando is straightforward once you know it's there and doesn't cause programs that use it to break once it's gone. But with bandwidth caps in place on most internet connections a program like this is basically spending your money for you without your knowledge. Pando (and to a lesser extent, Riot Games), I am disappointed in you. Knock it off please.

[![](/a/2011-11-14-the-flying-developer-dislikes-pando-media-booster/83458a6f-0df5-44e7-abbd-08058d61cd37.jpg)](http://cheezburger.com/View/4510849792)
