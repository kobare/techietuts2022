---
layout: post
title:  "Installing Ruby on Ubuntu Using rbenv"
author: Denis Kobare
date:   2023-01-26 10:30:00 +0300
img: /assets/img/svg/ruby.svg
categories: programming
sub_category: ruby
type: tutorials
technology: Ruby
permalink: "category/:categories/ruby/tutorials/:year:month/:title"
---


### Introduction
rbenv is a ruby version management tool designed to manage multiple versions of 
Ruby on the same device and lets you easily switch between them.



<br>
### 1 . Install dependencies for compiiling Ruby

{% highlight ruby %}

$ sudo apt-get install git-core curl zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev software-properties-common libffi-dev dirmngr gnupg apt-transport-https ca-certificates

{% endhighlight %}


<br>
### 2 . Install rbenv

{% highlight ruby %}

$ git clone https://github.com/rbenv/rbenv.git ~/.rbenv

$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc

$ echo 'eval "$(rbenv init -)"' >> ~/.bashrc

$ git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build

$ echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc

$ git clone https://github.com/rbenv/rbenv-vars.git ~/.rbenv/plugins/rbenv-vars

$ exec $SHELL

{% endhighlight %}


<br>
### 3 . Install Ruby 
Install a Ruby version of your choice. e.g 3.2.0

{% highlight ruby %}

$ rbenv install 3.2.0

# Make it the default version
$ rbenv global 3.2.0

# Confirm instllation
$ ruby -v

# ruby 3.2.0

{% endhighlight %}


<br>
### 4 . Install Bundler
This method is an alias for <span class="badge">inspect</span> method.

Returns the new String formed from each array element.

{% highlight ruby %}

# install the latest version
$ gem install bundler


# or you can install a version of your choice
$ gem install bundler -v 1.17.3


$ rbenv rehash


# See if it is installed

$ bundle -v

# Bundler version 2.3.26

{% endhighlight %}



<br>
*Thanks for reading, see you in the next one!*
