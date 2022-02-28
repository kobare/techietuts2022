---
layout: post
title:  "Install Devise on a Rails App"
author: Denis Kobare
date:   2022-02-28 16:30:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby/Rails
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---

### Definition

Devise is an authentication gem for rails applications. It allows you to create users that can log in and out of your application.


<br>
### 1. Add devise gem
 
a). Open up your Gemfile and add this line

{% highlight ruby %}
#/Gemfile

gem 'devise'
  
{% endhighlight %} 


and run:

{% highlight terminal %}

$ bundle install
	
{% endhighlight %} 	

Or:
	
b).Run:
{% highlight terminal %}

$ bundle add devise
 	
{% endhighlight %} 	





<br>
### 2. Set up devise in your app

{% highlight terminal %}
	
$ rails g devise:install

{% endhighlight %} 	


<br>
### 3. Configure Devise
Ensure you have defined default url options in your environments files. Open up config/environments/development.rb and add this line before the 'end' keyword:

{% highlight ruby %}
# /config/environments/development.rb
   
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
   
{% endhighlight %} 	


<br>
### 4. Add the notification alert
Open up app/views/layouts/application.html.erb and add the following right above <%= yield %> :

{% highlight html %}
 // app/views/layouts/application.html.erb
 
 <% if notice %>
  <p class="alert alert-success"><%= notice %></p>
 <% end %>
 <% if alert %>
  <p class="alert alert-danger"><%= alert %></p>
 <% end %>
 
{% endhighlight %} 	


<br>
### 5. Setup the User model

{% highlight terminal %}

$ rails g devise user
$ rake db:migrate

{% endhighlight %} 	


<br>
### 6. Add sign-up and login links. 
In order to do that go to, edit app/views/layouts/application.html.erb add:

{% highlight html %}

// app/views/layouts/application.html.erb

<p class="navbar-text pull-right">
 <% if user_signed_in? %>
  Logged in as <strong><%= current_user.email %></strong>.
  <%= link_to 'Edit profile', edit_user_registration_path, :class => 'navbar-link' %> |
  <%= link_to "Logout", destroy_user_session_path, method: :delete, :class => 'navbar-link'  %>
 <% else %>
  <%= link_to "Sign up", new_user_registration_path, :class => 'navbar-link'  %> |
   <%= link_to "Login", new_user_session_path, :class => 'navbar-link'  %>
  <% end %>
</p>

{% endhighlight %} 	


<br>
### 7. Redirect the User
Finally, force the user to redirect to the login page if the user was not logged in. Open up app/controllers/application_controller.rb and add the following after the protect_from_forgery with: :exception. :

{% highlight ruby %}

/controllers/application_controller.rb
 
 protect_from_forgery with: :exception

 before_action :authenticate_user!
  
{% endhighlight %} 


Make sure your rails server is running, open http://localhost:3000/users/sign_up and create your user account.	

<br>
### 8. Customize Devise Views

You can style the devise views according to your preference. Run the command below to generate the views and then provide css style for them:

{% highlight terminal %}

$ rails generate devise:views

{% endhighlight %} 


<br>
*Thanks for reading, see you in the next one!*
