---
layout: post
title:  "Deploying Rails App to AWS EC2 Ubuntu Server With Capistrano"
author: Denis Kobare
date:   2023-01-26 13:25:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: rails, capistrano
permalink: "category/:categories/ruby-on-rails/tutorials/:year:month/:title"
---


### Introduction

Deploying application updates takes multiple steps. Performing all these steps 
every time you want to deploy application updates is time-consuming and error-prone.
This section will guide you on how to automate the deployment of application updates 
through Capistrano.

Capistrano is a popular task automation tool among Ruby developers. Once Capistrano is set up,
deploying further application updates only takes a single command.


<br>
### Part A
ssh into your server and:

1. [Install Ruby](/category/programming/ruby/tutorials/202301/installing-ruby-on-ubuntu-using-rbenv){:target="_blank"}

2. [Install MySQL](/category/databases/mysql/tutorials/202301/installing-mysql-on-ubuntu){:target="_blank"}

3. [Install nginx and passenger](/category/more-topics/operating-systems/ubuntu/202301/installing-nginx-and-passenger-on-ubuntu){:target="_blank"}


<br> 
### Part B

### 1 . Initializing Capistrano
The first thing you need to do is to install Capistrano into your rails project. 


  i). Open the Gemfile and add:

{% highlight ruby %}

# Gemfile

group :production do
  gem 'capistrano'
  gem 'capistrano-bundler'
  gem 'capistrano-passenger', '>= 0.1.1'
  gem 'capistrano-rails'
  # Because we’re using rbenv on our EC2 Server
  gem 'capistrano-rbenv'
end

{% endhighlight  %}



<br>
  ii) Install the gem bundle and initialize Capistrano:


{% highlight terminal %}

$ bundle install

$ bundle exec cap install

{% endhighlight  %}



<br>
### 2 . Editing Capfile

Capfile is the Capistrano entry point. It defines what recipes to load. You must edit it to load the
recipes you need.

We will want Capistrano to automatically run bundle install, and we will want Capistrano to
automatically tell Passenger to restart our app. These are taken care of by the capistrano-bundler and
capistrano-passenger recipes. So make sure that the following lines are uncommented:

{% highlight ruby %}

# Capfile

#Because the server uses rbenv
require "capistrano/rbenv"

require "capistrano/bundler"
# require "capistrano/rails/assets" # hashed out because of server's assets precompile issues
require "capistrano/rails/migrations"
require "capistrano/passenger"

{% endhighlight  %}


<br>
### 3 . Editing config/deploy.rb

The next step is to edit config/deploy.rb. This file contains configuration values that control how the
loaded recipes should do their jobs. It also defines additional commands to be executed on servers. You
must edit it according to your situation.


{% highlight ruby %}
# config/deploy.rb


# config valid for current version and patch releases of Capistrano
# Remove the lock statement since already serves as a mechanism to lock down gem
# lock "~> 3.11.0"


# Set repo_url to your Git repository's URL
set :application, "myapp"
set :repo_url, "git@github.com:yourgitusername/myapp.git"


# Default deploy_to directory is /var/www/my_app_name
 set :deploy_to, "/home/username/projects/myapp"


# set linked_files and linked_dirs

set :linked_files, fetch(:linked_files, []).push('config/database.yml', 'config/secrets.yml', '.env')
set :linked_dirs, fetch(:linked_dirs, []).push('log', 'tmp/pids', 'tmp/cache', 'tmp/sockets', 'vendor/bundle', 'public/system', 'public/uploads')



namespace :deploy do

  after :restart, :clear_cache do
    on roles(:web), in: :groups, limit: 3, wait: 10 do
      # Here we can do anything such as:
      # within release_path do
      #   execute :rake, 'cache:clear'
      # end
    end
  end

end

{% endhighlight  %}


<br>
### 4 . Editing config/deploy/production.rb

The next step is to edit config/deploy/production.rb. This file defines the servers that Capistrano should
deploy to, in the form of SSH login information.

Our server is on Amazon EC2, so be sure to uncomment the set <span class="badge">:ssh_options</span> call and point the
keys option to your Amazon EC2 key file. Also set the auth_methods option to %w(publickey
password).

{% highlight ruby %}
# config/deploy/production.rb

# role-based syntax
# ==================

# Single server
# Replace with your server's IP and user
 role :app, %w{username@54.82.83.53}
 role :web, %w{username@54.82.83.53}
 role :db,  %w{username@54.82.83.53}


# Custom SSH Options
# ==================
# --------------
 set :ssh_options, {
  keys: %w(/home/username/somepath/your_key_pair.pem),
  forward_agent: false,
  auth_methods: %w(publickey password)
}


{% endhighlight  %}


<br>
### 5 . Setting up a basic directory structure
Run the following commands on the server to setup a basic directory structure that Capistrano can
work with.
 
{% highlight ruby %}

$ sudo mkdir -p /home/username/projects/myapp/shared
$ sudo chown myappuser: /home/username/projects/myapp /home/username/projects/myapp/shared

{% endhighlight  %}


<br>
### 6 . Create initial configuration files

The app expects a <span class="badge">config/database.yml</span> and a 
<span class="badge">config/secrets.yml</span> files. The contents of these 
configuration files are only known to the server and persist across
application releases. The shared directory is the perfect place to place them. 
And as configured before in <span class="badge">config/deploy.rb</span>, Capistrano will 
automatically create symlinks within the release directory to those files.


  i) On the server, create the <span class="badge">shared/config</span> directory and add <span class="badge">database.yml</span> and
<span class="badge">secrets.yml</span> files inside:

{% highlight terminal %}

$ sudo mkdir -p /home/username/projects/myapp/shared/config

$ sudo touch /home/username/projects/myapp/shared/config/database.yml && sudo touch /home/username/projects/myapp/shared/config/secrets.yml

{% endhighlight  %}


  <br>
  ii). Then fix and tighten permissions:

{% highlight terminal %}

sudo chown -R username: home/username/projects/myapp/shared/config
chmod 600 home/username/projects/myapp/shared/config/database.yml
chmod 600 home/username/projects/myapp/shared/config/secrets.yml

{% endhighlight  %}


  <br>
  iii). edit database.yml file

{% highlight yml %} 
  
production:
  adapter: mysql2
  encoding: utf8
  reconnect: false
  database: myapp_database
  pool: 5
  username: db_username
  password: db_password

{% endhighlight  %}


  <br>
  iv). On your development machine, generate the secret key and add it to 
       <span class="badge">config/initializers/secret_token.rb</span> file.

{% highlight terminal %} 
  
$ rails secret

{% endhighlight  %}


<br>
{% highlight ruby %} 
# config/initializers/secret_token.rb

# Be sure to restart your server when you modify this file.

# Your secret key is used for verifying the integrity of signed cookies.
# If you change this key, all old signed cookies will become invalid!

# Make sure the secret is at least 30 characters and all random,
# no regular words or you'll be exposed to dictionary attacks.
# You can use `rake secret` to generate a secure secret key.

# Make sure your secret_key_base is kept private
# if you're sharing your code publicly.
Rails.application.config.secret_key_base = '90a82db7702c2d0af8a5ad715962116b8fffaf7bb833162a32ef71d345845e424de9e0ecda1dbd81191201ecbtfc09c7f6a747eb17d7cc111490914df62b2c09'

{% endhighlight  %}


  <br>
  v). Add the secret key in the <span class="badge">secrets.yml</span> file on the server

{% highlight yml %}  
 
90a82db7702c2d0af8a5ad715962116b8fffaf7bb833162a32ef71d345845e424de9e0ecda1dbd81191201ecbtfc09c7f6a747eb17d7cc111490914df62b2c09  

{% endhighlight  %}
  
  
   
<br>
### 7 . Deploying a new release
You are now ready to deploy a new release using Capistrano!
On your local computer, make a random change in your application, then commit and push your changes.
Next, run Capistrano to start the deployment:

{% highlight terminal %}

$ bundle exec cap production deploy

{% endhighlight  %}


You have now automated deployments using Capistrano!

<br>
*Thanks for reading, see you in the next one!*
