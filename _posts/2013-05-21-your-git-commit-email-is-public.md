---
layout: post
title: "Remember: Your Git Commit Email is Public"
date: 2013-05-21 10:51
comments: true
categories:
---

Here's a fact I don't think a lot of people know. Or rather they do know, but haven't thought about:

__Your git commit email is public__

I spend a lot of my time on GitHub looking for talented developers to [work with me at Shopify](http://shopify.com/careers). In order to get in touch with them, I usually just click the email link on their profile.

When there isn't an email listed, I simply use GitHub's public API to get that person's recent public activity, and search for an '@' symbol. This yields a valid email in 100% of cases.

Here's my profile page as an example: [http://davefp.github.com](http://davefp.github.com)

Now here's my recent public activity: [https://api.github.com/users/davefp/events/public](https://api.github.com/users/davefp/events/public)

If you browse the JSON you'll actually get two addresses of mine: My personal email (davefp@gmail.com) and my work address (david.underwood@jadedpixel.com).

I'm not concerned with who has my email address. [I consider it to be public info anyway](http://theflyingdeveloper.com/why-i-dont-obfuscate-my-email/). But if you don't want all and sundry reading yours, consider using a fake address in your git identity.

__Update:__ [vsbuffalo](https://news.ycombinator.com/item?id=5745051) on [Hacker News](https://news.ycombinator.com/item?id=5744736) points out that GitHub has a help page to help you configure your commit email if you so desire: [https://help.github.com/articles/keeping-your-email-address-private](https://help.github.com/articles/keeping-your-email-address-private)