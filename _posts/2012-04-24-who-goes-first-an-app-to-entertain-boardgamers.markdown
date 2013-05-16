---
comments: true
date: 2012-04-24 12:00:24
layout: post
slug: who-goes-first-an-app-to-entertain-boardgamers
title: Who Goes First? An App to Entertain Boardgamers
wordpress_id: 981
categories:
- projects
post_format:
- Standard
tags:
- rails
- ruby
- web
---

I still consider myself to be a relative ruby/rails novice, so every now and again I build 'toy' projects to teach myself more about the framework. Usually these don't end up going anywhere, but my latest attempt over about 3 evenings of coding turned out rather well.


[![](/a/2012-04-24-who-goes-first-an-app-to-entertain-boardgamers/Screen-Shot-2012-04-24-at-12.24.26-PM-1024x269.png)](http://whogoesfirst.herokuapp.com)


[Who Goes First](http://whogoesfirst.herokuapp.com/) is a super-simple web app that spits out a random rule to decide who takes the first turn in a board game. It supports user submissions and has a contact form. That's it.


## Gems Used





	
  * [randumb](https://github.com/spilliton/randumb)

	
  * [state_machine](https://github.com/pluginaweek/state_machine)

	
  * [activeadmin](http://activeadmin.info)

	
  * [bootstrap-generators](https://github.com/decioferreira/bootstrap-generators)


The backbone of this app is [Active Admin](http://activeadmin.info/). I love this gem. The documentation took a little while to decipher but once I had a general idea of what was going on it was a pleasure to use. Before using activeadmin I'd always used the console to 'administer' my site - creating objects, updating state, that sort of thing. Activeadmin bypasses all of that by providing a proper web interface complete with authentication thanks to devise. It's just customizable enough to allow complex actions like triggering state changes on objects so it was perfect for Who Goes First as all user submissions need to be moderated before going live.


[![](/a/2012-04-24-who-goes-first-an-app-to-entertain-boardgamers/Screen-Shot-2012-04-24-at-12.26.08-PM-1024x269.png)](http://whogoesfirst.herokuapp.com)


[Randumb](https://github.com/spilliton/randumb) is a brilliant little gem that adds a `random` method to ActiveRelation that does exactly what you'd expect: Pulls and returns a random object from the current relation. I'm using it to grab a single object but you can specify a count if you want more. Very useful.

[Bootstrap-Generators](https://github.com/decioferreira/bootstrap-generators) provided my layouts. [Bootstrap](http://twitter.github.com/bootstrap/) (from Twitter) is a godsend for developers such as myself who hate spending time (and suck at) designing layouts. The generators gem gets everything set up for you and provides basic rails-y templates to get started with. One note if you're planning on using this gem though: At the time of writing the current release on rubygems.org does not contain the responsive templates, so grab it from the github repo instead. The responsive aspect was paramount for me as I envision people using the app on a smartphone, tablet or whatever at the gaming table.

Finally, there's [state_machine](https://github.com/pluginaweek/state_machine). I used this gem to provide the basis of my moderation mechanism. New user-submitted rules start in a 'pending' state and only appear once I move them to 'approved'. There's also a 'denied' state for rules I don't want published.


[![](/a/2012-04-24-who-goes-first-an-app-to-entertain-boardgamers/Screen-Shot-2012-04-24-at-12.27.07-PM-1024x270.png)](http://whogoesfirst.herokuapp.com)





## Other things I learned


[Heroku](http://heroku.com) has really good mail support thanks to [Sendgrid](http://sendgrid.com/). I was able to hook up my contact form really easily (although my webhost's mail server bounced the first few messages as the domains didn't match up. D'oh!).

Activeadmin doesn't play well with the asset pipeline. The styles and js are hidden away in the gem so I had to explicitly load them in the initializer. Easy once you know how, but it was a pain when I deployed to production for the first time and found that my styles were broken.


[![](/a/2012-04-24-who-goes-first-an-app-to-entertain-boardgamers/Screen-Shot-2012-04-24-at-12.28.00-PM-1024x270.png)](http://whogoesfirst.herokuapp.com)





## What's next?


I'm pretty pleased with the site as-is at the moment, but I do have some ideas for improvements. The first thing I want to do is add some normalization to new submissions so that they're all correctly capitalized and punctuated. Right now about half of the submissions have full stops at the end and half don't which and it's a pain to go in and edit them all manually.

I'm also considering adding modifiers to the rules. A lot of them refer to the 'most', 'biggest', or 'largest' of something. Detecting these and randomly replacing them with the opposite version ('least', 'smallest', etc.) would add some free variety to the ruleset.
