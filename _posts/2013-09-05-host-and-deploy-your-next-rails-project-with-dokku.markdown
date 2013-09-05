---
layout: post
title: "Host and deploy your next Rails project with Dokku"
date: 2013-09-05 10:05
comments: true
categories:
---

I built [jabcal.ca](http://jabcal.ca) over the weekend. To show it to the world, I did what I usually do: fire up a free Heroku dyno and `git push heroku master`.

A major component of JabCal is the calendar endpoint. It serves iCalendar files that are generated on the fly based on a starting date (and ideally more optional parameters soon). The idea is that you subscribe to the calendar using your favourite calendar app and if there are updates, you get them automatically.

Trouble is, Heroku puts your free dyno to sleep after an hour of inactivity. The first request to a sleeping dyno takes much longer to complete. Sometimes it times out. When there's a human on the other end that's acceptable as it only affects the first request they make, but a calendar app might ONLY make one request before giving up.

Of course, I could drop $35/month for a dedicated dyno to be kept online permanently, or cheat and use NewRelic or Pingdom to keep the dyno alive by pinging it periodically.

Neither of these solutions appealed to me, so I started researching alternate hosting solutions. I've used DigitalOcean before, but I'm lazy and didn't want to take the time to set up a new production environment manually.

Turns out there's a dirt simple way to set up a Heroku-like environment on a VPS which you can deploy to via git.

## Enter Dokku

Dokku is (as far as I can tell) a portmanteau of [Docker](https://www.docker.io/) and Heroku. The author [Jeff Lindsay](https://github.com/progrium) (also known for [localtunnel](http://progrium.com/localtunnel/)) calls it

> Docker powered mini-Heroku. The smallest PaaS implementation you've ever seen.

It's pretty badass. Here are the steps I took to get it running:

### Step 1

Create a new VPS instance. I started with a $5/month droplet on [DigitalOcean](https://www.digitalocean.com/?refcode=fd2b085e4355) as it's roughly equivalent to a single Heroku dyno. You can use your favourite VPS host, of course.

At this point I associated my public key with the droplet so that I never have to log in with the root password.

### Step 2

Log in and run this command to start the bootstrap script:

    wget -qO- https://raw.github.com/progrium/dokku/master/bootstrap.sh | sudo bash

Once it terminates, Dokku will be installed.

### Step 3

Add your public SSH key to git on your VPS so that you can push apps to it. This command (run on your local machine) should do the trick:

    cat ~/.ssh/id_rsa.pub | ssh 123.456.789.1 "sudo gitreceive upload-key user-name"

If you postpone this until after you have your domain name set up (Step 4) then naturally you can use that instead of an IP address for SSH.

### Step 4

Point a domain of your choice at your VPS. You'll need two A records: One for the root and one for a wildcard. This is because Dokku uses the root domain to receive git pushes and subdomains to host the apps you create.

If you skip this step then each app will be assigned a different port on your host to run out of, which is sucky and bad so don't do it :(

### Step 5

Add your domain to `/home/git/VHOST` so that Dokku knows where you're hosted. Again, skipping this will result in deployed apps getting a port number rather than a subdomain.

### Step 6

Finally, I installed the [postgres plugin](https://github.com/Kloadut/dokku-pg-plugin) so that Rails would run. There's an extra mini step here because you have to explicitly create the pg database BEFORE pushing your app, but that's as simple as running `dokku postgresql:create jabcal`

### Deployment

Now all that remains is to push your code. You don't even need to explicitly create the app on the server, Dokku automatically creates it on the fly based on the name of the remote you push to. For example:

    git remote add dokku git@dokku.theflyingdeveloper.com:jabcal

followed by

    git push dokku master

Is all you need to do to get things running. In my case, the app became available at `jabcal.dokku.theflyingdeveloper.com`.

One really neat thing about Dokku is that it uses the same build-packs as Heroku. Since my app was already happily deployed there, it worked flawlessly the first time it was deployed on Dokku. 

I basically ended up with something roughly equivalent (for my purposes, anyway) to a single Heroku dyno that would have cost $35/month for a mere $5/month. I'd highly recommend it for anyone building a semi-serious side project for whom Heroku's free plan is inadequate.

## The really exciting part

Dokku is written in less than 100 lines of bash. That's not the exciting part though. Jeff and an old colleague of mine [Jonathan](http://twitter.com/titanous) are having a crack at writing a new, better version of Dokku designed to be used by ops teams at tech companies to host their own fully-fledged private PaaS infrastructure.

The project is called [Flynn](https://flynn.io/) and it's currently in development. I think it's going to be a huge boon to devs, as it'll allow firing up apps for things like staging, internal tools, experiments, and all kinds of other things, whenever they like, with zero deployment hassle. That's seriously exciting.