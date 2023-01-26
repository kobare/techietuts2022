---
layout: post
title:  "Rails Generator Cheatsheet"
author: Denis Kobare
date:   2022-12-01 06:00:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: cheatsheet
technology: Ruby
permalink: "category/:categories/ruby-on-rails/cheatsheet/:year:month/:title"
---


### Definition

Rails generator takes so much of the work out of the developer’s hands which 
makes it a great tool for a more experienced developer but not the best one for a beginner.

Here are some of the most useful generators to help speed up development process.

<br>
### 1. Scaffold

The <span class="badge">scaffold</span> creates all the necessary components with 
the attributes supplied. It creates the migration, model, controller, view, css, route and tests files.

It is worth noting that the <span class="badge">resource</span> generator is similar 
to scaffolding. The only difference is that it does not generate the view template 
files and controller actions. It creates an empty view folder and a controller without actions. 

{% highlight terminal %}

# rails g scaffold <name of model> <model attributes>
$ rails g scaffold Person name age:integer address


# To generate a namespaced resource (API in this case) do:
# NB: skips the model and migration
$ rails g scaffold_controller api/v1/persons name:string age:integer address:string  --api --model-name=Person

{% endhighlight  %}



<br>
### 2. Controller

The <span class="badge">controller</span> generator generates both the controller and the view templates.

{% highlight terminal %}

# rails g controller <name of controller> <controller actions>
$ rails g controller person index show create destroy


# To generate the controller only do:
$ rails generate controller person index show create destroy --skip-template-engine

{% endhighlight  %}



<br>
### 2. Model

The <span class="badge">model</span> generators create a model file as well as the database migration file. 

{% highlight terminal %}

# rails g model <name of model> <model attributes>
$ rails g model Person name age:integer address


# To generate the model only (skip migration) do:
$ rails g model Person name age:integer address --skip-migration 

or:

$ rails g model Person name age:integer address --migration=false


# To create a new model with a reference to another model do;
$ rails g model Citizenship name person:references country:references


# To generate a model that has a polymorphic reference do:
$ rails g model Comment body:text commentable:references{polymorphic}:index  

{% endhighlight  %}



<br>
### 3. Migration

The <span class="badge">migration</span> generator generates the migration file.

{% highlight terminal %}

# rails g migration <name of migration> <model attributes>
$ rails g migration create_table_person name age:integer address

{% endhighlight  %}


<br>
*Thanks for reading, see you in the next one!*
