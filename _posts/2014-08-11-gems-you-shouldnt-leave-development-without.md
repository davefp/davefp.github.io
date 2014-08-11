---
layout: post
title: "Gems You Shouldn't Leave Development Without"
date: 2014-08-11 11:00
comments: true
categories:
---

A couple of weeks ago I gave a talk at [Ottawa Ruby](http://ottawaruby.ca) called 'Gems you Shouldn't Leave Development Without'.

You can find the video and slides below, as well as notes adapted from the slides:

<iframe class="video-embed" src="//www.slideshare.net/slideshow/embed_code/37252271" frameborder="0" allowfullscreen></iframe>

<iframe class="video-embed" src="//www.youtube.com/embed/goxmQsj-PkY" frameborder="0" allowfullscreen></iframe>


# Development Gems!

## quiet_assets <small>[link](http://rubygems.org/gems/quiet_assets)</small>

Makes assets shut the hell up in development.


## better_errors <small>[link](http://rubygems.org/gems/better_errors)</small>

Adds a much better error page for development.

Also a REPL if you add `binding_of_caller` too!

## pry + pry-debugger <small>[link](http://rubygems.org/gems/pry)</small>

Pry is a great alternative to IRB.

Pry-debugger adds your favourite debug commands:

* step
* next
* continue
* finish

Allows you to set breakpoints too.

# OK, Let's Talk Production

## Environment Management

### figaro <small>[link](http://rubygems.org/gems/figaro)</small>

Figaro is a rails-specific environment management gem.

### dotenv <small>[link](http://rubygems.org/gems/dotenv)</small>

Dotenv is similar to Figaro but works with any rack-based ruby app.

## Error Reporting

### exception_notification <small>[link](http://rubygems.org/gems/exception_notification)</small>

Dirt simple error notification. Send errors by email, or to another service (e.g. group chat) using various adapters.

### Exception Notification Services

* Airbrake
* Errbit
* Honeybadger
* Rollbar
* Lots more

Ruby Toolbox has a great list: [https://www.ruby-toolbox.com/categories/exception_notification](https://www.ruby-toolbox.com/categories/exception_notification)

## Application Server

* WEBrick
* Thin
* Unicorn
* Passenger
* Puma

Go with Unicorn unless you have a reason not to.

Don't use WEBrick in production.

## How About... Middleware!

### rack-canonical-host <small>[link](http://rubygems.org/gems/rack-canonical-host)</small>

Redirect all requests to a specific host. Can also be used to force SSL. Great for custom domains on Heroku.

Supports tons of options including regex matching on host strings.

### rack-attack <small>[link](http://rubygems.org/gems/rack-attack)</small>

Connection whitelists, blacklists, and throttling. Stops misbehaving clients from even hitting your app code.

Works well with Fail2Ban blacklists and others.

### rack-google-analytics <small>[link](http://rubygems.org/gems/rack-google-analytics)</small>

Adds an analytics code to every page of your app.

## Heroku

### rails_12factor <small>[link](http://rubygems.org/gems/rails_12factor)</small>

Tells Rails to serve assets. This is usually delegated to the routing webserver (e.g. nginx or apache).

Redirects logs to stdout rather than logfiles.

Stops Heroku complaining every time you deploy.