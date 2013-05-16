---
comments: true
date: 2011-04-11 10:00:47
layout: post
slug: how-to-write-an-app-in-8-hours-or-die-trying
title: How To Write An App In 8 Hours (Or Die Trying)
wordpress_id: 421
categories:
- tutorials
tags:
- hack days
- ottawa
- waterloo
---

[![Scene from Hack Ottawa](/a/2011-04-11-how-to-write-an-app-in-8-hours-or-die-trying/5486139417_0be4d2e33d.jpg)](http://www.flickr.com/photos/60078087@N04/5486139417/in/photostream/)On April 9th I attended [Hack Waterloo](http://hackdays.ca/category/hackwaterloo/), a day-long event where 40 developers managed to build 12 apps supported by half a dozen APIs in less than 8 hours. I love these events so much that I travelled down from Ottawa on Friday night to attend. Waterloo was my second hack day, and it was definitely my most successful. I didn't win any prizes, but I did have something to demo at the end of the day which was a first for me. I'll leave a fuller break-down of the event itself to other more qualified people, but I'd like to share my thoughts and ideas on how to be successful at events like this (that is to say, have something that can be demoed at the end of the day. Beating the competition is up to you!).


## Preparation, Preparation, Preparation


Due to the travel time required to attend, I had a four-and-a-half hour period of forced solitude on the journey down. Fortunately Via Rail provides free WiFi on their trains. I used this opportunity to pin down as much information and do as much set-up as possible for the next day. This fell into three broad categories.


### Framework/Environment


On the day of the event the last thing you want to do is have to spend the first hour or two downloading packages, installing programming environments or setting up code repositories. Make sure you have a clear idea of the technologies and platforms you want to work with and get them set up. It's worth trying to pick something that you have at least a little experience with otherwise most of the day will be spent browsing Stack Overflow. In a team environment this is trickier as you might not know who you're working with until you show up. In a lot of cases though different parts of the project can be handed off to team-members who have the most experience in that area.

On the topic of teams, if you're working in a group you'll need some sort of code repo to share your work. The tool of choice these days is git, so make sure you know how to use it and have it installed. Github is a great hosting choice if you don't mind open-sourcing your project, but if the network you're on co-operates you can share code directly between team-members in a peer-to-peer fashion (this is one reason why a distributed versioning tool like git is better than a centralized solution like svn for these types of events). The morning of the first hack day I attended was basically spent getting everyone on our team familiar with git/github which meant less time for coding.

If you're attending an event based on a particular technology and have access to documentation beforehand, for crying out loud **read it**. At the bare minimum do the Hello World tutorial at home a few days before to make sure that you have the basics covered.


### Domain Specific Tools


[![Scene from Hack Ottawa](/a/2011-04-11-how-to-write-an-app-in-8-hours-or-die-trying/5486734612_db171381a8-171x300.jpg)](http://www.flickr.com/photos/60078087@N04/5486734612/in/photostream/)One of the core tenets of good software development is code-reuse. Unless you're dealing with a super-brand-new technology/platform, the chances are that someone has developed tools to make your life easier. Many APIs have client libraries for various languages, both first and third-party. Even if they don't there are are a whole host of general-purpose libraries that provide nice interfaces for the various types of http request, as well as processing the responses. In particular I'd recommend having a solid library for making multipart POST requests (for file upload) and decent tools for encoding/parsing both xml and json. Because of the tight time constraints you want to write as little code as possible so having a wide toolset is essential.


### Supporting Data


The particular project I was working on required a lot of structured data to produce decent results, including images. In a production environment the source of this data would be provided by the client, but for the prototype I had to resort to whatever I had on my hard-drive. If I'd thought about it in advance I could have prepared some source data or maybe even written a script to automate the process. This kind of preparation might not be applicable in your case, but it's something that is easily overlooked.


## At The Event




### Iterate Early, Iterate Often


[![Scene from Hack Ottawa](/a/2011-04-11-how-to-write-an-app-in-8-hours-or-die-trying/5486734886_29f4b7d778-199x300.jpg)](http://www.flickr.com/photos/60078087@N04/5486734886/in/photostream/)If you don't have a demo, you don't have anything. On more than one occasion I've worked on a project all day, hacking away on one section at a time and being hugely disappointed when I ran out of time before being able to tie it all together and making it work. For this reason, it is essential that you have **something** that runs as soon as possible. It does't matter if it has hard-coded data, does exactly the same thing no matter what the user enters, or looks like it was drawn by a five-year-old: Once you have something that runs, you have a demo. Even if you screw everything else up you can still revert to this state at the end of the day. Similarly, your project should still run after you've implemented each new feature.


### Don't Be Afraid To Ask


One of the things I like about Hack Days is that all the featured APIs have representatives available either at the event or remotely to help you out with their platform. Often times these are the people actually responsible for writing the API and as such they have extensive technical knowledge about it. If you have a problem trying to interface with their platform, they'll be more than happy to help you out. After all, they benefit as much as you from having a working app created atop their data.

Even if you have more general problems, don't forget that you're in a room full of hackers! Someone there has probably dealt with an issue similar to yours before and will be able to help you out. At Hack Waterloo I was couldn't get Rails to recognise a dependency I'd just installed. It was in my gemfile, I'd run the bundle installer, and I was out if ideas. I asked around and quickly found a more experienced ruby hacker who told me that I needed to restart my rails server for the change to take effect (new dependencies are only loaded on startup). In a time-restricted environment it's incredibly frustrating to have to waste time fixing little issues like this. I'm really glad I asked before spending too long trying to figure it out.


### Have Fun!


I admit that this a huge cliché, but it is important nonetheless. Don't forget that hack events are supposed to be fun. Spend some time over lunch or after everyone has finished coding chatting with people, socializing and generally having a good time (There might even be beer, if you're into that sort of thing). If you're working in a team, be nice to your team-mates. I guarantee you'll have a better time if you get along with them instead of arguing.


## Try, Try Again


[![](/a/2011-04-11-how-to-write-an-app-in-8-hours-or-die-trying/5491438717_3036f51137-208x300.jpg)](http://www.flickr.com/photos/iwatchlife/5491438717/in/photostream/)As with all things, practice makes perfect. If your first app is a flop for whatever reason, don't be disheartened. Look back at the day and see what worked well and what could use improvement. See when the next event near you is happening, and start preparing for it.

If you're a coder who lives in Canada, I highly recommend [Hack Days](http://hackdays.ca), who run events all across the country. So far they've  been to Toronto, Montreal, Ottawa, and Waterloo and have upcoming events in Vancouver and even an entire event that will take place on a train between Montreal and Toronto! Maybe I'll see you there.

(All images in this post were taken at Hack Ottawa in February and are used with permission from [YellowAPI.com](http://yellowapi.com) under a creative-commons license)
