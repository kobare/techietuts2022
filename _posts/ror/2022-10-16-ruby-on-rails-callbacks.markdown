---
layout: post
title:  "Ruby on Rails Callbacks"
author: Denis Kobare
date:   2023-10-16 03:33:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: concepts
technology: Ruby
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---

### Definition
Callbacks are methods that are called at some points of an object's lifecycle. 
They are always called before particular controller actions (usually listed on top of the file). 
The reason for this is that you need to find an object from the database and assign 
it to an instance variable in all those actions. 

A callback method is meant to keep the code DRY (Do not Repeat Yourself), because if you 
don't have a private method for this, you would have to copy/paste the line inside 
the method for each controller action. That is fine as long as you don't need to 
change it EVER. That is why it is good practice to use callbacks for these kinds of methods.


<br>
### Part A: Creating a Rails 7 Project 

Create a new rails project called <span class="badge">callbacks_example</span> by following this document: [Creating A Rails 7 App: With esbuild, bootstrap and jquery](/category/frameworks/ruby-on-rails/tutorials/202204/creating-a-rails-7-app){:target="_blank"}.


<br>
### Part B: Creating the models

1 . Create the <span class="badge">person.rb</span> and <span class="badge">country.rb</span> models

 i). Generate the models
 
{% highlight terminal %}

 $ rails g model person
 
 $ rails g model country

{% endhighlight %}
 
   


<br>
*Thanks for reading, see you in the next one!*
