---
layout: post
title: "Controller-Specific Assets with Rails 4"
date: 2014-06-11 12:58
comments: true
categories:
---

**Update**: I gave a talk on this subject at the June 2014 meetup of [Ottawa Ruby](http://ottawaruby.ca). I've embedded the screencast below (sorry about the poor audio in places):

<iframe class="video-embed" src="//www.youtube.com/embed/4j0CmcTTjZc" frameborder="0" allowfullscreen></iframe>

**Original Post**

Whenever you generate a controller in Rails, you're given (amongst other things) a couple of asset files:

```
/app
  /assets
    /stylesheets
      controller_name.css
    /javascripts
      controller_name.js.coffee
```

The initial content of these files reads:

```
# Place all the behaviors and hooks related to the matching controller here.
# All this logic will automatically be available in application.js.
# You can use CoffeeScript in this file: http://coffeescript.org/
```

Fantastic! Rails is giving you a place to place controller -specific styles and scripts that will run when an action from that controller is run.

Except... Not quite.

## The Problem

The truth is that while these asset files allow you to organize your code a little better, their content is all lumped together into application.js/application.css and loaded into every page.

This is easy to see in development by opening your browser's developer tools on any of your app's pages and looking at the resources being loaded:

![controller specific assets 1.png](https://draftin.com:443/images/16026?token=di3M7nOBCoHtVfVix2A3ny4NDK8uWAOUYtuhqzLwWEKiZDvABfbXl0GQwFYpdyrz5oG1Hv44rncYWCqgiahOkwM)

All the files outlined above are controller asset files. If you have conflicting styles or script code between these files, it can lead to some very strange problems.

The reason this happens is that by default, application.js and application.css both have the `require_tree .` directive. That means they'll include every asset in the directory tree starting at the root asset dir. This includes all the controller-specific asset files.

## The Fix

Fortunately there are some relatively straightforward changes you can make to change this behaviour.

### Step 1: Remove the tree directive

Let's stop Rails from loading all our assets into every page.

Simply remove the `//= require_tree .` line from application.js and the ` *= require_tree .` line from application.css.

### Step 2: Include Controller-specific assets in each page

Now we're going to tell our pages to load the relevant assets.

In your application layout (application.html.erb by default) update the asset include lines to read:

```
<%= stylesheet_link_tag "application", params[:controller], :media => "all" %>

<%= javascript_include_tag "application", params[:controller] %>
```
This loads the application-wide asset file as before, but also loads an asset file with the same name as the current controller. Cool!

### Step 3: Tell Rails to compile our controller assets

By default, the asset pipeline only compiles application.js/.css and any files they require. Because we removed the `require_tree .` directive, we need to tell Rails about our controller-specific assets or they won't get compiled.

Create a new initializer file called `config/initializers/assets.rb` and add the following (replacing the controller names with your own):

```
%w( controller_one controller_two controller_three ).each do |controller|
  Rails.application.config.assets.precompile += ["#{controller}.js.coffee", "#{controller}.css"]
end
```

This loop simply iterates over all the controller names and adds the css and js to the list of assets Rails should compile.

That's it! You can now write whatever styles and scripts you like in your controller css and js files without worrying about conflicts occurring. Hooray!