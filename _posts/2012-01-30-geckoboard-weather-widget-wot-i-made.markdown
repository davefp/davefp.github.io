---
comments: true
date: 2012-01-30 10:00:28
layout: post
slug: geckoboard-weather-widget-wot-i-made
title: Geckoboard Weather Widget (Wot I Made)
wordpress_id: 913
categories:
- projects
tags:
- geckoboard
- rails
- weather
---

Earlier this week I came across [Geckoboard](http://www.geckoboard.com/), a neat app that allows you to create [Panic-like status boards](http://www.panic.com/blog/2010/03/the-panic-status-board/) to monitor all sorts of interesting stats.


[![](/a/2012-01-30-geckoboard-weather-widget-wot-i-made/geckoboardLogo_500_53.png)](http://geckoboard.com)


I decided I wanted to use it to create a living room dashboard that pulls in my calendar events, the current time, the weather, and maybe a todo/shopping list. I would then place my iPad somewhere prominent and have all the info available at a glance.




## Mixing Business and Pleasure


Unfortunately for me, most of Geckoboard's built in widgets are integrations with business oriented services: [Github](http://github.com), [Basecamp](http://basecamphq.com/), [Pingdom](http://www.pingdom.com/), that sort of thing. Whilst these would look great in an office lobby, I don't think my wife would appreciate live uptime stats in our living room. Time to improvise!

Geckoboard has this concept of custom widgets which allows you to shoehorn any data you can get your hands on into various predefined templates for display. There are a range of different [chart types](http://support.geckoboard.com/entries/274940-custom-chart-widget-type-definitions) available, as well as [maps, numerical displays, and plain text](http://support.geckoboard.com/entries/231507-custom-widget-type-definitions).

I decided I was going to get my feet wet by creating a weather widget that would display the current temperature along with the conditions. This would be represented in a custom plain text widget.

Sourcing the weather data turned out to be really easy. Yahoo has a [fantastic weather API](http://developer.yahoo.com/weather/) which is free to use for non-commercial purposes that works by simply sending a request with your location to a particular endpoint. The hardest part (which is still dead simple) is looking up your 'WOEID' (short for [Where On Earth ID](http://developer.yahoo.com/geo/geoplanet/guide/concepts.html)), a numeric ID for your location.


## WOE Is Me


With my WOEID in hand, it was time to wrangle the data. I created a new Rails project with a simple endpoint that took an ID and spat out the weather data for that location. With a little help from the [xml-simple gem](http://xml-simple.rubyforge.org/) I then refined the response to include only the data I needed (The temperature and the condition text) before re-wrapping the response in json to be returned. This was maybe half a dozen lines of code in my controller. In fact, Rails is probably way over the top for something like this but it's what I have the most experience with so I went with it anyway.

I created an app on Heroku to host everything, and then tried it out. Here's what the widget looks like (excuse the boring Ottawa weather):

[![](/a/2012-01-30-geckoboard-weather-widget-wot-i-made/geckoboard_weather_widget1.png)](http://theflyingdeveloper.com/blog/wp-content/uploads/2012/01/geckoboard_weather_widget1.png)

Really simple, but exactly what I wanted. I liked it so much that I decided to share it. Head over to the app on Heroku for instructions on how to use it yourself: [Gecko-Weather](http://gecko-weather.heroku.com/).

I'm planning on adding a couple of extra features soon, namely the ability to specify Fahrenheit/Celsius and some error handling in case the weather info is unavailable for some reason. I'm also open to suggestions, so please post any requests in the comments!


