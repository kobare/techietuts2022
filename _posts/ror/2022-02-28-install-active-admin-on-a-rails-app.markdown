---
layout: post
title:  "Install Active Admin on a Rails App"
author: Denis Kobare
date:   2022-02-28 17:48:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby/Rails
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---

### Definition

Active Admin is a Ruby on Rails gem for generating administration interfaces.


<br>
Make sure [Devise](/category/frameworks/ruby-on-rails/concepts/202202/install-devise-on-a-rails-app) is installed and properly configured.
### 1. Add the gem
 
Open up your Gemfile and add these lines.
NB: This has been tested with Rails 7 ☑️

{% highlight ruby %}
#/Gemfile

# Use activeadmin
gem 'activeadmin', github: 'tagliala/activeadmin', branch: 'feature/railties-7' # FIXME: revert to stable
gem 'arbre', github: 'activeadmin/arbre' # FIXME: remove
gem 'inherited_resources', github: 'activeadmin/inherited_resources' # FIXME: remove

# Use sass-rails
gem 'sass-rails'

{% endhighlight %} 


and run:

{% highlight terminal %}

$ bundle install
	
{% endhighlight %} 	


<br>
### 2. Set up activeadmin in your app

{% highlight terminal %}
	
$ rails generate active_admin:install

$ rails db:migrate

{% endhighlight %} 	


<br>
### 3. Visit http://localhost:3000/admin and log in using:

    User: admin@example.com
    Password: password


<br>
### 4. Register a model
To register your first model, run:



{% highlight html %}

$ rails generate active_admin:resource MyModel

{% endhighlight %} 


<br>
*Thanks for reading, see you in the next one!*
