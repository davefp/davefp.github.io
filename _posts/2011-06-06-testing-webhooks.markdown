---
comments: true
date: 2011-06-06 10:00:00
layout: post
slug: testing-webhooks
title: Testing Webhooks
wordpress_id: 556
categories:
- tutorials
---

[Webhooks](http://www.webhooks.org/) are an emerging feature in many great APIs. They allow you (or rather, your applications) to avoid polling a service for data by pushing events to you as they happen. However, this aspect makes them tricky to test outside of a production-ready environment. Fortunately, there is a really simle method that makes consuming webhooks in a development environment a breeze.


## The Problem


Whereas most API calls are initiated by your application, webhooks originate from the API provider's own servers. Because of this role reversal, an app's testing environment  must be publicly accessible if webhooks are being used and tested. One solution is  to use a free hosting solution such as a single [Heroku](http://www.heroku.com/) dyno or [App Engine](http://code.google.com/appengine/) to  temporarily run the app while you test. I did this for a while but it  didn't suit my iterative development process as every time I made  a change I had to re-deploy the app. So how does one test features that  require a publically visible server from behind a firewall or network  gateway?


## The Solution


Enter [localtunnel](http://progrium.com/localtunnel/). Localtunnel is a free service that creates an  [SSH tunnel](http://en.wikipedia.org/wiki/Tunneling_protocol) from your machine to the outside world using a custom url on  their site. All this is done behind the scenes by a Ruby gem so you  don't know the first thing about SSH in order to get it working. Here's  how you set it up:

**Prerequisites**: You'll need [ruby](http://www.ruby-lang.org/en/) and [rubygems](http://rubygems.org/) installed to take advantage of localtunnel.




  1. Open a console and install the localtunnel gem:


    gem install localtunnel





  2. If you don't have one, set up a public/private keypair. On Linux or  Mac, open a terminal and run


    ssh-keygen


Accept all the defaults. On Windows it's a bit trickier but your best bet is to install [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/) and follow the instructions  here: [http://kb.siteground.com/article/How_to_generate_an_SSH_key_on_Windows_using_PuTTY.html](http://kb.siteground.com/article/How_to_generate_an_SSH_key_on_Windows_using_PuTTY.html)


  3. The first time you run localtunnel you'll need to tell it about your public key, followed by the port you wish to forward:


    localtunnel -k ~/.ssh/id_rsa.pub 3000





  4. Each subsequent time you can  just specify the port you wish to expose to the outside world:


    localtunnel 3000





  5. Copy the url that is returned into your app as the base for your webhook handler addresses:


    Port 3000 is now publicly accessible from
    http://44ea.localtunnel.com ...





  6. Register your webhooks with the service you're using.


That's it! You can now test webhooks  or other push features to your heart's content. When you're done, just  terminate the localtunnel process to close the connection. The one downside is that the address localtunnel provides changes each time you run it, so don't rely on it staying the   same between sessions.
