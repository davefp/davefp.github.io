---
comments: true
date: 2009-05-02 22:02:38
layout: post
slug: flex-3-bouncing-marquee
title: Flex 3 Bouncing Marquee
wordpress_id: 73
categories:
- projects
tags:
- actionscript
- coding
- flex
---

I was working on a secret flex project (Shh!) the other week that required some scrolling text. "I wonder how to implement that in flex?" I thought. I've been working with flex [at work](http://macadamian.com) for about 9 months now, but I've never encountered the need for such a component before.

A quick Google search revealed this: [Marquee Component](http://butterfliesandbugs.wordpress.com/2007/09/06/marquee-component/). Fantastic! One of my favourite things about working with flex is that there's a huge community out there who have already written the component you need in 90% of cases.

Still, I needed to adapt the component to fit my needs. The 'secret project' involves a media player, and the marquee was for the song title. I wanted it to display the artist and title at a fixed width, but scroll back and forth if the text was too long. I call it a 'bounce' marquee. [Here's the demo](http://theflyingdeveloper.com/code_examples/bounce_marquee/BounceMarqueeDemo.html).

The first modification I made was to change the way in which the component defined the movement. The original marquee had a property defining the duration of the scrolling. This was unsuitable because it meant that longer text scrolled faster. Make the string long enough, and the text would scroll past too fast to read. Instead, the bouncing marquee has a scrollSpeed property that defines the speed in pixels per second. The length of the string (in pixels) is divided by this number to produce the duration of the scroll, which is then passed to the Move effect.

Secondly I changed the effects to match my desired behaviour. I simply defined to Move effects with Pause effects between inside a Sequence. The first move takes the text from it's initial position to and scrolls it until the end of the string can be seen. The second move takes it back. The length of the pauses can be changed by overriding the pauseDuration property on the component.

This was all well and good, but there was still a problem. Updating the properties of an Effect whilst it's playing does not take effect until the effect is stopped and restarted. Therefore I needed a public update() method to be called that would reset the effects and allow the changes to take place.

All in all I'm pretty pleased with how the component turned out. The hardest part was figuring out why the effects weren't updating properly when I changed the text. It seems there was a timing issue that judicious application of the validateNow() method fixed.

