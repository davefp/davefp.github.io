---
layout: post
title: "Nginx, OmniAuth, and SSL: A debugging tale"
date: 2014-01-27 21:13
comments: true
categories:
---

While developing [https://webhookbook.com](https://webhookbook.com) I ran into a really strange issue. In production, my OAuth calls to GitHub were failing with an `invalid_credentials` message from OmniAuth.

Looking for a potential cause, I was disappointed that OmniAuth was only providing a simple `invalid_credentials` error with no further message. To get more info, I dived into my nginx logs.

Lo and behold, the message coming back from GitHub identified the issue as a 'callback URL mismatch'. This meant that the URL I'd supplied them when creating the app didn't match the one my app was requesting at runtime. Looking at the URL params, I saw the callback being sent was `http://webhookbook.com`. Notice the insecure protocol. Huh.

My entire site is behind SSL, so why is Omniauth sending an insecure callback URL? Time for some more digging.

Fortunately OmniAuth's codebase has excellent test coverage. In the `strategy_spec.rb` file, I found numerous references to `callback_url` as well as `full_host`.

Both of these looked interesting, so I opened the `Strategy` class to examine them. Thanks to a [helpful comment](https://github.com/intridea/omniauth/blob/master/lib/omniauth/strategy.rb#L418), I found that OmniAuth makes a best-effort attempt to determine if the callback URL should be secure or not based on a number of different request headers. Here's the code:

``` ruby
# sometimes the url is actually showing http inside rails because the
# other layers (like nginx) have handled the ssl termination.

def ssl?
  request.env['HTTPS'] == 'on' ||
  request.env['HTTP_X_FORWARDED_SSL'] == 'on' ||
  request.env['HTTP_X_FORWARDED_SCHEME'] == 'https' ||
  (request.env['HTTP_X_FORWARDED_PROTO'] && request.env['HTTP_X_FORWARDED_PROTO'].split(',')[0] == 'https') ||
  request.env['rack.url_scheme'] == 'https'
end
```

Bingo. Nginx was doing the ssl termination, and I wasn't passing any of the right headers in my nginx config, so Rails (and OmniAuth) had no idea that I was behind a secure connection. Adding an appropriate header solved the problem immediately.

## An Aside about Staging Environments

Interestingly (and annoyingly), the failure didn't present in my staging environment as it wasn't behind SSL. I'd bought a certificate for my root domain so that I could run the production site under https, but my staging site was on a subdomain and so wasn't covered.

This was a bit of a blow because well, what's a staging environment for if not to catch errors that are specific to the real-life deployment environment?

Ultimately having that staging server did help, as I could immediately eliminate all the identical areas of the deployment as possible causes (database, environment variables, DNS, etc.) and narrow the issue to something SSL related fairly quickly.

The moral of the story is this: Don't cheap out on your staging environment. It needs to resemble production as closely as possible otherwise things like this will slip through.

If you don't have a staging environment for a project that's already in production, set one up ASAP or you'll be looking at a [Toilet Paper Problem](http://theflyingdeveloper.com/toilet-paper-problems) down the line.