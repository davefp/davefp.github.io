---
layout: post
title: "Migrating Hubot from Heroku to Dokku"
date: 2015-04-10
comments: true
categories:
---

With the potential demise of Heroku's free 24/7 dyno on the horizon, I decided now was the time to move my hubot installation onto another platform.

I decided to go with Dokku running on a 512MB Digital Ocean droplet. Dokku offers the same push-to-deploy workflow as Heroku, so once everything was set up the day-to-day operation of the app would be identical.

## Step 1: Set up a VPS

Dokku requires Ubuntu 14.04 x64 to run, so spin up a virtual server on your favourite provider with that installed.

Personally I'm a fan of Digital Ocean, and they provide an image with the latest version of Dokku pre-installed, so I went with that. If you go with a vanilla OS install, follow the set-up instructions here: [http://progrium.viewdocs.io/dokku/installation](http://progrium.viewdocs.io/dokku/installation)

It's also worth pointing a domain at your new server at this point otherwise you'll have to use the IP address to connect every time, which is a pain.

## Step 2: Deploy Hubot

Add a new remote to your existing hubot repo called 'dokku' that points at your server:

    git remote add dokku dokku@example.com:hubot

The important parts here are:

* The remote's name should be `dokku`
* The user it connects to your server with should also be `dokku`
* The repo name (the text after the `:`) should be the name of the application. It will be used as the subdomain, which in this example would result in `hubot.example.com`.

Now you can push to that remote just like you would with Heroku and hubot will start on your new server. Hooray!

## Step 3: App Configuration

Now that hubot is deployable, you should configure it. To do this with Dokku, you need to ssh into your server as the `dokku` user and run commands. Note that this user is set up to run the `dokku` command with the arguments you pass to ssh, then exit. Hence, simply typing:

    ssh dokku@example.com

will display the help for dokku then exit.

Grab the Heroku config for your app using `heroku config`. You can now put these into dokku using the `config:set` dokku command. It'll look something like this:

    ssh dokku@example.com config:set hubot KEY1=VALUE1 KEY2=VALUE2

Importantly, if your hubot uses the redis brain plugin, copy across the redis url variable and you'll still be able to use the redis instance you were using on Heroku. I have a free 5MB RedisToGo instance created through their Heroku plugin that I still use from my Dokku deploy.

## Step 4: Server Tweaks

By default, Dokku will wait 60 seconds after deploying an app before shutting the old one down. This is to allow long-running requests to finish but a) hubot requests aren't that long and b) hubot will respond twice to commands sent during that time.

To change the default timeout, we need to set the default timeout environment variable on the server itself (rather than in the app). Ubuntu has a few places to do this but my preference is to set it system-wide.

To do this ssh into your server *as root* and add the following line to `/etc/environment`

    DOKKU_WAIT_TO_RETIRE=2

Now Dokku will only wait 2 seconds before retiring the old version of the app. Much better!

Finally, running hubot on 512MB of RAM is perfectly fine but can cause issues during deploy while both the old and new instances are running.

To prevent OOM errors, you can enable some swap space that will temporarily carry the memory load during deploys. There are many guides on doing this, but here's a good one specific to Ubuntu 14.04: [https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04](https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04)

## Summary

Here's what we just did:

* provision a VPS with Dokku pre-installed
* Deploy our hubot app for the first time
* Pull config vars from Heroku and pass them to Dokku
* Reduce the overlap time when deploying new versions
* Add swap space so that we don't run out of memory while deploying
