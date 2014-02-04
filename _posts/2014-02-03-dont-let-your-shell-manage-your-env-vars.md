---
layout: post
title: "Don't let your shell manage your env vars: Use Dotenv or Figaro instead"
date: 2014-02-03 15:34
comments: true
categories:
---

## Problem

During the development of [https://webhookbook.com](https://webhookbook.com), I changed the SMTP credentials. Because I'm a good developer I was storing my credentials in environment variables, so I ssh'd into my production server, updated `.bashrc` with the new values, and restarted the server. Done, right?

Not done. It turned out that because I was defining my env vars in my `.bashrc` and performing 'hot' restarts of my Unicorn server (using the `kill -s USR2 unicorn_pid` method) the environment was never getting reloaded, so the old credentials were still in use. Oops!

## Solutions

Fortunately, there are a number of gems out there to help you with this problem.

Dotenv (<https://github.com/bkeepers/dotenv>) and Figaro (<https://github.com/laserlemon/figaro>) are very similar gems that both provide the ability to add environment variables in a config file. They parse said file during app initialization and load the stored variables into the `ENV` hash.

The main concrete difference I can see (aside from using differently named yml files) is that Figaro allows you to access ENV vars via the `Figaro.env` object as well as the `ENV` hash. The argument here is that it's easier to stub out `Figaro.env` than a raw hash during testing.

Personally I'm using Dotenv at the moment, but you can basically flip a coin to pick the one you want to use.

There are other solutions available that provide settings management in Rails, but I prefer Dotenv and Figaro because they hook into the existing environment rather than requiring me to access a different object to get my settings. They allow you to access configuration uniformly throughout the app, even if you're also relying on externally defined environment variables.

As a bonus, updating my env vars is now easier. I can just scp a new version of the file over to the production box before restarting the server. No need to manually edit pesky remote files. Yay!
