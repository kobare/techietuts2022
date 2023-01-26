---
layout: post
title:  "Install Rails Admin on a Rails App"
author: Denis Kobare
date:   2022-02-28 17:28:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby/Rails
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---

### Definition

Rails Admin is a Rails engine that provides an easy-to-use interface for managing your data. It is used to generate administration interfaces just like Active Admin, although Rails Admin automatically generates admin views for your models. Most developers prefer to be more hands-on and therefore lean more towards [Active Admin](/category/frameworks/ruby-on-rails/concepts/202202/install-active-admin-on-a-rails-app).



<br>
Make sure [Devise](/category/frameworks/ruby-on-rails/concepts/202202/install-devise-on-a-rails-app) is installed and properly configured.
### 1. Add the gem
 
Open up your Gemfile and add these lines.

{% highlight ruby %}

# /Gemfile

### NOTE: If you are using Rails 4, you do not need to add the first Gem (remotipart).
gem 'remotipart', github: 'mshibuya/remotipart'
gem 'rails_admin', github: 'sferik/rails_admin'
gem 'rails_admin_rollincode', '~> 1.0'
gem "devise"
	
{% endhighlight %} 


and run:

{% highlight terminal %}

$ bundle install
	
{% endhighlight %} 	


<br>
### 2. Set up Rails Admin in your app

{% highlight terminal %}
	
$ rails g rails_admin:install

$ rails db:migrate

{% endhighlight %} 	

Rails Admin will ask on which route you would like to install, leave /admin as the default, but you can choose another route.

<br>
### 3. Administer Your Data
Fire up the rails server and visit http://localhost:3000/admin to administer your data


<br>
### 4. Install Rails Admin Theme 
The rails admin theme provides news colors, adjustments and a brand new tree view menu.

i). Open Gemfile and add the following gems:

{% highlight ruby %}
# /Gemfile

gem 'rails_admin_rollincode', '~> 1.0'
gem 'rails_admin', git: 'https://github.com/sferik/rails_admin.git'
	
{% endhighlight %} 


<br>
ii). Inside config/application.rb, just after Bundler.require:

{% highlight ruby %}
# config/application.rb

# Bundler.require
ENV['RAILS_ADMIN_THEME'] = 'rollincode'	
	
{% endhighlight %} 


<br>
iii). You'll have to run theses commands for changes to take effect:

{% highlight terminal %}

rake assets:clean && rake assets:precompile	                
                OR	                
rm -rf tmp/cache/assets/development/
	                
{% endhighlight %} 


<br>
iv). Add JavaScript<br>
We can't include custom js in a bundled theme with rails_admin for now, you have to add in your app/assets/javascripts/rails_admin/custom/ui.js the following code : for the javascript menu to work:


{% highlight javascript %}
// app/assets/javascripts/rails_admin/custom/ui.js

	$(document).on('ready pjax:success', function() {
	  handleActiveBase();
	  function handleActiveBase() {
	    $('.sub-menu').each(function () {
	      if ($(this).hasClass('active')) {
		$(this).parent().prev().addClass('active');
		$(this).parent().prev().addClass('open');
		$(this).parent().slideDown();
	      }
	    });
	  }
	});

	$(function () {
	  var width = $('.nav-stacked').width();
	  $('.navbar-header').width(width);

	  var array_menu = [];
	  var lvl_1 = null;
	  var count = 0;

	  $('.sidebar-nav li').each(function (index, item) {
	    if ($(item).hasClass('dropdown-header')) {
	      lvl_1 = count++;
	      array_menu[lvl_1] = []
	    } else {
	      $(item).addClass('sub-menu sub-menu-' + lvl_1);
	    }
	  });

	  for (var i = 0; i <= array_menu.length; i++) {
	    $('.sub-menu-' + i).wrapAll("<div class='sub-menu-container' />");
	  }

	  $('.sub-menu-container').hide();

	  handleActiveBase();
	  function handleActiveBase() {
	    $('.sub-menu').each(function () {
	      if ($(this).hasClass('active')) {
		$(this).parent().prev().addClass('active');
		$(this).parent().slideDown();
	      }
	    });
	  }

	  $('.dropdown-header').bind('click', function () {
	    $('.dropdown-header').removeClass('open');
	    $(this).addClass('open');

	    $('.dropdown-header').removeClass('active');
	    $('.sub-menu-container').stop().slideUp();
	    $(this).toggleClass('active');
	    $(this).next('.sub-menu-container').stop().slideDown();
	  });
	});	

{% endhighlight %} 

<br>
*Thanks for reading, see you in the next one!*
