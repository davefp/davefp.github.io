---
layout: post
title: "Ubuntu, Nginx, Unicorn, Rails, Capistrano, rbenv, Oh My!"
date: 2014-05-26 12:45
comments: true
categories:
---

## Foreword

Here are the steps I use when creating new servers to host my apps on. Both [arenagym.net](http://arenagym.net) and [webhookbook.com](https://webhookbook.com) are set up this way.

### Why not automate this?

One day I'm sure I will. For now though, I still have a lot to learn. Getting my hands dirty is my preferred way to do this.

### Is this a tutorial?

Not really. I don't claim any of the things I do are best practices, or even secure. I've cherry-picked the things that worked for me from other sources and mashed them together so that I can reference them in future.

##Spin up an instance

Your choice: AWS, Digital Ocean, Linode, etc.

I'm using Ubuntu 14.04.

##Get SSH access

This varies by provider, but I recommend adding an authorized key and logging in with that, rather than a password.

Digital Ocean lets you select from pre-uploaded keys to install, making the process straightforward.

## Install prerequisites

```bash
root@remote $ apt-get update
root@remote $ apt-get install curl git postgres postgres-contrib libpq-dev git-core zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libxml2-dev libxslt1-dev nodejs nginx
```

Details on why each of these packages are needed can be found at the bottom of this post.

##Create deploy user

[Source](http://capistranorb.com/documentation/getting-started/authentication-and-authorisation/)

From an account with root access:

```bash
root@remote $ adduser deploy
root@remote $ passwd -l deploy
```

Give the deploy user an authorized key

```bash
root@remote $ su - deploy
deploy@remote $ cd ~
deploy@remote $ mkdir .ssh
deploy@remote $ echo "some public key" >> .ssh/authorized_keys
deploy@remote $ chmod 700 .ssh
deploy@remote $ chmod 600 .ssh/authorized_keys
```

## Configure Postgres

[Source](https://help.ubuntu.com/community/PostgreSQL)

Set up a db account for the deploy user, and a database for your app.

```bash
root@remote $ sudo -u postgres createuser --superuser deploy
root@remote $ sudo -u postgres create database <db-name>
```

## Install rbenv (and ruby-build)

[Source](https://github.com/sstephenson/rbenv#installation)

```bash
deploy@remote $ git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
deploy@remote $ git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
deploy@remote $ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
deploy@remote $ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
```

Restart your shell.

Install a ruby:

```bash
deploy@remote $ rbenv install <favourite-ruby-version>
```

## Set up app server-side

Make all the dirs

```bash
deploy@remote $ mkdir ~/apps
deploy@remote $ mkdir ~/apps/<app-name>
deploy@remote $ mkdir ~/apps/<app-name>/shared
```

Create unicorn log dir

Copy over any linked files, e.g.

```bash
user@local $ scp config/database.yml deploy@server-ip:~/apps/<app-name>/shared/config/database.yml
```

If you're using dotenv:

```bash
deploy@remote $ touch ~/apps/<app-name>/shared/.env
```

Install bundler

```bash
deploy@remote $ gem install bundler
```

Create Devise secret key

deploy@remote $ echo "DEVISE_SECRET_KEY=xxxxxxxxxxxxx" >> ~/apps/<app-name>/shared/.env

Create other environment variables in the same fashion (Rails secret, API keys, etc.)

Create nginx config for your site

```nginx
upstream app_server {
  server unix:/tmp/unicorn.<app-name>.socket fail_timeout=0;
}

server {
  listen 80;
  server_name <app-domain>;

  root /home/deploy/apps/<app-name>/current/public;

  location / {
    proxy_set_header  X-Real-IP  $remote_addr;
    proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_redirect off;

    if (-f $request_filename/index.html) {
      rewrite (.*) $1/index.html break;
    }

    if (-f $request_filename.html) {
      rewrite (.*) $1.html break;
    }

    if (!-f $request_filename) {
      proxy_pass http://app_server;
      break;
    }
  }
}
```

If you're using SSL, The following will allow you to redirect all insecure requests to the secure version of your site (i.e. HTTP -> HTTPS)

```nginx
server {
  listen 80;
  server_name <app-domain>;
  return 301 https://$host$request_uri;
}

server {
  listen 443 ssl;
  server_name <app-domain>;
  ssl_certificate     /etc/nginx/ssl/<app-host>.chained.crt;
  ssl_certificate_key /etc/nginx/ssl/<app-host>.key;

  root /home/deploy/apps/<app-name>/current/public;
  # rest of your config goes here as normal
}
```

## Required packages

* **curl** - HTTP tool. Used by all sorts of things
* **git** - Installs git, which we'll need to deploy our app
* **postgres** - The database for our app
* **postgres-contrib** - Required for the postgres gem to build
* **libpq-dev** - Header files for linking to postgres
* **build-essential** - Required to build native gems
* **zlib1g-dev** - Gzip implementation
* **libssl-dev** - Part of OpenSSL, needed for ssl
* **libreadline-dev** - Rails dependency
* **libyaml-dev** - YAML support
* **libxml2-dev** - XML support
* **libxslt1-dev** - More XML support
* **nodejs** - JS runtime
* **nginx** - Webserver for routing requests to Rails