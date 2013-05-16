---
comments: true
date: 2009-02-16 17:22:35
layout: post
slug: reactions-plugin-update
title: Reactions Plugin Update
wordpress_id: 18
categories:
- wp-reactions
tags:
- reactions
- wordpress
---

The Wordpress 'reactions' plugin that I'm working on is coming along well. In fact, I actually have a demo! Take a peek at my [testing blog](http://test.theflyingdeveloper.com) to see it in action. You can check any of the boxes next to the reaction text (cool, interesting and boring) and see the number of ratings updated.

The plugin itself is still not ready for public release. If you look at the demo, you'll see that you can un-check a box after checking it and the counter will still go up. There's a whole bunch of other stuff that needs changing too:



	
  * The reaction text is hard coded. I need to write an admin page that allows the user to specify the reactions they want to display and some extra php to actually display the correct text on the page.

	
  * As mentioned above, the UI needs some work. Specifically, the check boxes need to be grayed-out after being clicked to to prevent users spamming the system.

	
  * It would be nice to have some sort of check performed to see whether a given user has already rated a post and correctly display this information on return visits.

	
  * I would eventually like to have an admin tracking page that can be used to display the highest ranked posts for each response. I would also like to implement a widget that can be used to display this information to visitors if desired.


The last two points probably won't make it into the first public release, as they aren't essential to the functionality of the feature. The first two are essential though, and hopefully I'll get a chance to work on them sometime this week.
