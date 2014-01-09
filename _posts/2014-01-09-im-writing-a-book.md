---
layout: post
title: "I'm Writing a Book"
date: 2014-01-09 15:50
comments: true
categories:
---

**TL;DR:** I'm writing a book on how to handle webhooks in Rails. It's called **Healthy Webhook Consumption with Rails** and you can buy it here for $10: [https://webhookbook.com](https://webhookbook.com)

---

While I was working at Shopify I spent a lot of time helping third-party developers learn how to use our API. In particular, I wrote a fair bit about webhooks. Here are a couple of examples:

* [Webhook Best Practices](http://www.shopify.com/technology/5154662-webhook-best-practices)
* [Webhook Testing Made Easy](http://www.shopify.com/technology/3931292-webhook-testing-made-easy)

Webhooks are a fantastic tool, but I've not been able to find an authoritative extended guide on the subject. Every provider has just enough documentation to get you up and running, but like with any programming topic there's a lot you can/should do to make your app more elegant and failure-resistant.

To make matters worse there's no official spec for webhooks, so everyone implements them slightly differently.

This is why I've decided to write a book for developers building webhook-consuming apps. It's in the early stages at the moment (Only the introduction has been committed to paper) but over the next couple of months I'm going to be adding to it rapidly covering everything from your first webhook subscription to the nuances and differences between webhook implementations.

Just to make things more interesting, I also built the book's website ([webhookbook.com](https://webhookbook.com)) from scratch. I'm actually very proud of it, it's the first site of mine that I've deployed into an environment that I configured from the ground up. There's a follow-up blog post in the works about all the cool things I learned in the process.