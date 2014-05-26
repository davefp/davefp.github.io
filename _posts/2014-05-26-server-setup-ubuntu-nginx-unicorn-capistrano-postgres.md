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

```bash
deploy@remote $ echo "DEVISE_SECRET_KEY=xxxxxxxxxxxxx" >> ~/apps/<app-name>/shared/.env
```

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

## Deployment config

Capfile:

```ruby
require 'capistrano/setup'

require 'capistrano/deploy'

require 'capistrano/rbenv'

require 'capistrano/rails'

require 'capistrano3/unicorn'

Dir.glob('lib/capistrano/tasks/*.cap').each { |r| import r }
```

deploy.rb:

```ruby
# config valid only for Capistrano 3.1
lock '3.2.1'

set :application, 'app-name'
set :repo_url, 'git@wherever.git'

set :linked_files, %w{config/database.yml .env}
set :linked_dirs, %w{tmp/pids}

set :unicorn_config_path, "config/unicorn.rb"

set :rbenv_type, :user # or :system, depends on your rbenv setup
set :rbenv_ruby, '2.1.1'
set :rbenv_prefix, "RBENV_ROOT=#{fetch(:rbenv_path)} RBENV_VERSION=#{fetch(:rbenv_ruby)} #{fetch(:rbenv_path)}/bin/rbenv exec"
set :rbenv_map_bins, %w{rake gem bundle ruby rails}
set :rbenv_roles, :all # default value

namespace :deploy do

  desc 'Restart application'
  task :restart do
    on roles(:app), in: :sequence, wait: 5 do
      invoke 'unicorn:restart'
    end
  end

  after :publishing, :restart

  after :restart, :clear_cache do
    on roles(:web), in: :groups, limit: 3, wait: 10 do
      # Here we can do anything such as:
      # within release_path do
      #   execute :rake, 'cache:clear'
      # end
    end
  end

end
```

production.rb:

```ruby
set :stage, :production

set :deploy_to, '~/apps/app-name'

set :branch, 'master'

set :rails_env, 'production'

# Simple Role Syntax
# ==================
# Supports bulk-adding hosts to roles, the primary
# server in each group is considered to be the first
# unless any hosts have the primary property set.
role :app, %w{deploy@app-host}
role :web, %w{deploy@app-host}
role :db,  %w{deploy@app-host}
```

unicorn.rb:

```ruby
# Set environment to development unless something else is specified
env = ENV["RAILS_ENV"] || "development"


# Production specific settings
if env == "production"
  app_dir = "app-name"
  worker_processes 4
end

# listen on both a Unix domain socket and a TCP port,
# we use a shorter backlog for quicker failover when busy
listen "/tmp/unicorn.#{app_dir}.socket", :backlog => 64

# Preload our app for more speed
preload_app true

# nuke workers after 30 seconds instead of 60 seconds (the default)
timeout 30

# Help ensure your application will always spawn in the symlinked
# "current" directory that Capistrano sets up.
working_directory "/home/deploy/apps/#{app_dir}/current"

# feel free to point this anywhere accessible on the filesystem
user 'deploy', 'deploy'
shared_path = "/home/deploy/apps/#{app_dir}/shared"

stderr_path "#{shared_path}/log/unicorn.stderr.log"
stdout_path "#{shared_path}/log/unicorn.stdout.log"

pid "#{shared_path}/tmp/pids/unicorn.pid"


before_fork do |server, worker|
  # the following is highly recomended for Rails + "preload_app true"
  # as there's no need for the master process to hold a connection
  if defined?(ActiveRecord::Base)
    ActiveRecord::Base.connection.disconnect!
  end

  # Before forking, kill the master process that belongs to the .oldbin PID.
  # This enables 0 downtime deploys.
  old_pid = "#{shared_path}/pids/unicorn.pid.oldbin"
  if File.exists?(old_pid) && server.pid != old_pid
    begin
      Process.kill("QUIT", File.read(old_pid).to_i)
    rescue Errno::ENOENT, Errno::ESRCH
      # someone else did our job for us
    end
  end
end

after_fork do |server, worker|
  # the following is *required* for Rails + "preload_app true",
  if defined?(ActiveRecord::Base)
    ActiveRecord::Base.establish_connection
  end
end
```

## Deploy!

You should now be able to deploy your app:

```bash
user@local $ cap production deploy
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