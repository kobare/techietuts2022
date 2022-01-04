---
layout: post
title:  "Ruby on Rails Database Environment Variables"
author: Denis Kobare
date:   2022-01-04 16:30:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: concepts
technology: Ruby/Rails
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---

### Definition

Environment variables are configuration variables stored in the computer for applications to access. You can configure environment variables on your computer for all applications to access, but this article will only focus on setting up these variables for Rails applications.

As a developer, you want to store database configuration variables i.e user name and password securely with env vars rather than ‘hard-coding’ the keys into your rails app, to avoid giving away such information when you push projects to VCS repositories online. Environment variables help you achieve that.

<br>
### Setting Up Environment Variables For the Database in a Rails App

1 . In your rails project, create a  ruby file in /config directory that will contain the ENV variables.

   e.g my_secret.rb, which will contain this:

{% highlight ruby %}
# /config/my_secret.rb

ENV['MYSQL_USERNAME'] = 'admin'
ENV['MYSQL_PASSWORD'] = 'complexpassword'

{% endhighlight %} 

<br>
2 . You want the rails app to load my_secret.rb file as soon as the server fires up. To do that, you will use the File and load methods inside the /config/environment.rb file in the rails project. Make sure to place this code just above the line: Rails.application.initialize!

{% highlight ruby %}
# /config/environment.rb

# load my_secret.rb
app_credentials = File.join(Rails.root, 'config', 'my_secret.rb')
load(app_credentials) if File.exist?(app_credentials)

# Initialize the Rails application.
Rails.application.initialize!

{% endhighlight %}  

<br>
3 . Ensure the my_secret.rb file is not submitted to github whenever you commit your project to a github repository. To do that, open the .gitignore file and add the path to the file as shown below:

{% highlight text %}
# .gitignore

/config/my_secret.rb

{% endhighlight %} 

<br>
With the these steps you are done setting up the environment variables for your database. Now if you open the rails console and enter this ENV['MYSQL_USERNAME'], it should output the content of that variable

{% highlight rails-console %} 

$ ENV['MYSQL_USERNAME']

$ admin

$ ENV['MYSQL_PASSWORD'] 

$ complexpassword

{% endhighlight %}

<br>

### Accessing the ENV Variables in database.yml file

In the database.yml file, you can now access the ENV variables and not have to 'hardcode' the credentials in there.

{% highlight yml %} 

default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: <%= ENV["MYSQL_USERNAME"] %>
  password: <%= ENV["MYSQL_PASSWORD"] %>
  socket: /var/run/mysqld/mysqld.sock

development:
  <<: *default
  database: my_app_development

test:
  <<: *default
  database: my_app_test

{% endhighlight %}


<br>
Note that you can easily achieve this using a gem like figaro, but it's always good to know how to do things by yourself to enrich your understanding of how bundled solutions work.

*Thanks for reading, see you in the next one!*
