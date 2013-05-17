---
layout: post
title: "Basic Rails Security: Lessons from IceBox"
date: 2012-11-06 21:58
comments: true
categories:
---

Last week I read an article titled [Never Give Your Information To 10 Minute Old Startups](http://blog.ryankearney.com/2012/10/never-give-your-information-to-10-minute-old-startups/) on the terrible security holes [IceBox](http://iceboxpro.com) had left open in their app.

Specifically, they had urls like this: `/users/12` for people's dashboards. Changing the user id gave you full access to another person's account. Oops!

There are a couple of really simple things that the devs at IceBox should have done before letting the public access this page:

1. Fixed their URLs
1. Applied some authorization logic

I don't know what stack IceBox is running behind the scenes, but I'm going to show you how to do these things in Rails.

<!-- more -->

##URLs

A lot of the time, you want to make resources available at a specific URL. Blog posts, wiki pages, or any other browsable resource. Should users be browsable? Probably not.

In rails, the default RESTful routes (created by adding `resources :things` to your routes file) assume plurality. Product**s**, comment**s**, article**s**, and so on. To access a particular instance of a resource, you hit `/users/:id`, `/users/:id/edit`, and so on. Cool!

But what if a user should only have access to a particular instance of a resource? How to I get a URL like this: `/user`?

Rails supports the use of sigular resources to solve this exact problem. Instead of using `resources :users` to generate the routes, you simply use `resource :user`. Note the lack of pluralization. This gives you URLs like `/user/new` and `/user`.

Whenever a user hits one of these singular resources, it's up to the app to decide which resource gets loaded. Which brings us neatly to...

##Authorization

Security revolves around two things: Authentication and authorization. These terms are often used interchangably, but there are important differences. In laymans terms:

* When a user logs in, you **authenticate** them
* When a user trys to perform an action, you **authorize** them

The failure IceBox made was in authorization. Any logged-in user could access the account of any other user.Fixing the URL structure as described above goes part-way to preventing this issue, but you still need to figure out which user account to display (if any!) when someone hits your site.

In all my Rails projects, I use a gem called [CanCan](https://github.com/ryanb/cancan) to solve this problem. CanCan is a fantastic authorization framework which hides much of the plumbing and lets you focus on defining the abilities of various user roles.

There are many, many tutorials on adding CanCan to your project. I recommend starting with the [official documentation](https://github.com/ryanb/cancan#cancan--) and then watching the [CanCan Railscast](http://railscasts.com/episodes/192-authorization-with-cancan). I'm going to show a simple example for loading the correct user when someone hits the `/user` endpoint.

First up, open the `ability.rb` file that the CanCan install creates and add the following:

{% highlight ruby %}
class Ability
  include CanCan::Ability

  def initialize(user)
    can :manage, User, :user_id => user.id
  end
end
{% endhighlight %}

That code tells CanCan that the logged in user (passed in through the `user` param) has full access to `User` objects that have their id.

CanCan assumes that you have a `current_user` method available in your application controller. The simplest version of this method might do this:

{% highlight ruby %}
def current_user
  user.find(session[:current_user_id])
end
{% endhighlight %}

How the session is set up falls under **authentication**, so we won't deal with it here.

The final piece is to set up your User controller to check that the person making the request is authorized to do so. CanCan has a handy helper method called `load_and_authorize_resource` that creates an instance variable with the same name as the controller **after** checking the current user's abilities against the defined rules.

{% highlight ruby %}
UserController < ApplicationController
  load_and_authorize_resource

  def show
    # Nothing to do here!
  end
end
{% endhighlight %}

We can leave the show method empty and the @user variable gets passed to our view for display. Yay! Now people can only access their own accounts.

##Conclusion

The IceBox example is a real-world case demonstrating what can happen when you rush the deployment of your app.

Hopefully the tips above will stop the same fate befalling your next app.