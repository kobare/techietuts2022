---
layout: post
title:  "Setting Up Email For Rails Devise Authentication"
author: Denis Kobare
date:   2022-08-01 05:55:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby/Rails/Devise
permalink: "category/:categories/ruby-on-rails/tutorials/:year:month/:title"
---

### Introduction

There are several ways to authenticate users on a Rails app and the easiest approach is to use existing tools such as the <span class="badge">Devise</span> gem, since it's a mature solution that has thousands of hours of code review, design, testing, and time in production.

During the authentication process, your app needs to commmunicate to the users via email because features such as <span class="badge">password reset</span> and <span class="badge">account confirmation</span> are contingent on the email service. Rails has an inbuilt component called <span class="badge">Action Mailer</span> that allows your app to send emails with just a few tweaks. 

This section reveals the simple configurations needed to allow Devise to send account confirmation request and password reset link emails to app users.

<br>
### Prerequisites

_NB: You may skirt these requirement versions if you are willing to experiment._


1. Ruby 3.2.0
2. Rails 7.0.3
3. Gmail account



<br>
### Part A: Creating a Rails 7 Project 

Create a new rails project called <span class="badge">devise_email</span> by following this document: [Creating A Rails 7 App: With esbuild, bootstrap and jquery](/category/frameworks/ruby-on-rails/tutorials/202204/creating-a-rails-7-app){:target="_blank"}.


<br>
### Part B: Installing Devise

1 . Add <span class="badge">devise</span> gem in the <span class="badge">Gemfile</span> file.

{% highlight ruby %}
# Gemfile

gem 'devise'
  
{% endhighlight %}


<br>
2 . Bundle install

{% highlight terminal %}

$ bundle install
  
{% endhighlight %}


<br>
3 . Setup devise

{% highlight terminal %}

$ rails g devise:install

{% endhighlight %}


<br>
4 . Generate <span class="badge">user</span> model

{% highlight terminal %}

$ rails g devise user

{% endhighlight %}


<br>
5 . Add <span class="badge">confirmable</span> and <span class="badge">recoverable</span> to the <span class="badge">users</span> table 
in <span class="badge">db/migrations/[...]_devise_create_users.rb</span>.

{% highlight ruby %}
# db/migrations/[...]_devise_create_users.rb

# frozen_string_literal: true

class DeviseCreateUsers < ActiveRecord::Migration[7.0]
  def change
    create_table :users do |t|
      ## Database authenticatable
      t.string :email,              null: false, default: ""
      t.string :encrypted_password, null: false, default: ""

      ## Recoverable
      t.string   :reset_password_token
      t.datetime :reset_password_sent_at

      ## Rememberable
      t.datetime :remember_created_at

      ## Trackable
      t.integer  :sign_in_count, default: 0, null: false
      t.datetime :current_sign_in_at
      t.datetime :last_sign_in_at
      t.string   :current_sign_in_ip
      t.string   :last_sign_in_ip

      ## Confirmable
      t.string   :confirmation_token
      t.datetime :confirmed_at
      t.datetime :confirmation_sent_at
      #t.string   :unconfirmed_email # Only if using reconfirmable

      ## Lockable
      #t.integer  :failed_attempts, default: 0, null: false # Only if lock strategy is :failed_attempts
      #t.string   :unlock_token # Only if unlock strategy is :email or :both
      #t.datetime :locked_at


      t.timestamps null: false
    end

    add_index :users, :email,                unique: true
    add_index :users, :reset_password_token, unique: true
    add_index :users, :confirmation_token,   unique: true
    #add_index :users, :unlock_token,         unique: true
  end
end

{% endhighlight %}


<br>
6 . Add <span class="badge">devise :confirmable</span> in <span class="badge">models/user.rb</span> file.

{% highlight ruby %}
# models/user.rb
# ...
devise :confirmable

{% endhighlight %}


<br>
7 . Run migrations

{% highlight terminal %}

$ rails db:migrate

{% endhighlight %}


<br>
8 . Add <span class="badge">:turbo_stream</span> as a navigational format in 
<span class="badge">config/initializers/devise.rb</span>. Rails 7 throws this error if you dont have it:
<span class="badge">undefined method `user_url' for #<Devise::RegistrationsController:0x0000000000cf08></span> 

{% highlight ruby %}
# config/initializers/devise.rb
# ...

config.navigational_formats = ['*/*', :html, :turbo_stream]

{% endhighlight %}


<br>
9 . Redirect user to login page if they are not signed in.
 
Add this code to <span class="badge">controllers/application_controller.rb</span> before any other actions.

{% highlight ruby %}
# controllers/application_controller.rb

  before_action :authenticate_user!
# ...

{% endhighlight %}



<br>
10 . Add <span class="badge">sign in</span> and <span class="badge">log out</span> 
links to the navbar in <span class="badge">app/views/layouts/application.html.erb</span>

{% highlight html %}
<!-- views/layouts/application.html.erb -->
<!-- .... -->  

  <body>

   <nav class="navbar navbar-expand-sm bg-dark navbar-dark fixed-top">
    <div class="container-fluid">
     <a class="navbar-brand" href="#">Devise Email Example</a>
     <a class="navbar-brand" href="#"></a>
     <% if current_user.nil? %>
      <%= link_to "sign in", new_user_session_path, method: :get, class: "btn btn-primary btn-sm"%>
     <% else %>
      <%= link_to "log out", destroy_user_session_path, method: :delete, class: "btn btn-primary btn-sm"%>
     <% end %>        
    </div>
   </nav>
  
   <% flash.each do |key, value| %>
    <div class="<%= flash_class(key) %>">
     <%= value %>
    </div>
   <% end %>
    <%= yield %>
  </body>

{% endhighlight %} 


<br>
### Part C: Setting Up Gmail App Passwords

<span class="badge">ActionMailer</span> will use your gmail account to send mail, but you can not use your normal gmail password with external applications. Gmail has a nifty feature called [App passwords](https://myaccount.google.com/apppasswords){:target="_blank"} that lets you sign in to your Google Account from apps on devices that don't support 2-Step Verification. Click [here](https://myaccount.google.com/apppasswords){:target="_blank"} to set up App passwords. On the page, select 'Mail' under the 'select app' tab, then enter a custom name under the 'select device' tab.

Click on the 'Generate' button to generate the password. Just like your normal password, this app password grants complete access to your Google Account. Copy 16-character password shown, you will need it for the next step.


<br>
### Part D: Setting Up Environment Variables For the Email Credentials

1 . In your rails project, create a  ruby hidden file in /config directory that 
will contain the ENV variables. Prepend a dot <span class="badge">.</span> in the file name to make it hidden.

   e.g <span class="badge">.my_secret.rb</span>, which will contain this:

{% highlight ruby %}
# /config/.my_secret.rb

ENV['EMAIL_USER_ID'] = 'your_id@gmail.com'
ENV['EMAIL_PASSWORD'] = 'the_gmail_app_password'

{% endhighlight %} 


_NB: press <span class="badge">Ctrl + H</span>to view hidden files on linux._


<br>
2 . You want the rails app to load <span class="badge">.my_secret.rb</span> file as soon as the server fires up. To do that, you will use the File and load methods inside the /config/environment.rb file in the rails project. Make sure to place this code just above the line: <span class="badge">Rails.application.initialize!</span>

{% highlight ruby %}
# /config/environment.rb

# load my_secret.rb
app_credentials = File.join(Rails.root, 'config', '.my_secret.rb')
load(app_credentials) if File.exist?(app_credentials)

# Initialize the Rails application.
Rails.application.initialize!

{% endhighlight %}  

<br>
3 . Ensure the <span class="badge">.my_secret.rb</span> file is not submitted to github whenever you commit your project to a github repository. To do that, open the .gitignore file and add the path to the file as shown below:

{% highlight text %}
# .gitignore

/config/.my_secret.rb

{% endhighlight %} 

<br>
With the these steps you are done setting up the environment variables for your database. Now if you open the rails console and enter this <span class="badge">ENV['EMAIL_USER_ID']</span>, it should output the content of that variable:

{% highlight rails-console %} 

$ ENV['EMAIL_USER_ID']

$ your_id@gmail.com

$ ENV['EMAIL_PASSWORD'] 

$ the_gmail_app_password

{% endhighlight %}


<br>
### Part E: Configure Devise to work with <span class="badge">ActionMailer</span>

1 . Add your email address in <span class="badge">config/initializers/devise.rb</span>

{% highlight ruby %}
# config/initializers/devise.rb
# ...
config.mailer_sender = 'your_id@gmail.com'

config.reconfirmable = false

{% endhighlight %}


<br>
2 . Configure <span class="badge">ActionMailer</span> for the development 
environment in <span class="badge">config/environments/development.rb</span>. 

{% highlight ruby %}
# config/environments/development.rb
# ...
  config.action_mailer.raise_delivery_errors = true

  config.action_mailer.perform_caching = false
  
  config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
  config.action_mailer.delivery_method = :smtp
  config.action_mailer.smtp_settings = {
    :address => "smtp.gmail.com", 
    :port => 587,   
    :domain => "gmail.com",
    :tls => true,          
    :enable_starttls_auto => true,
    :authentication => :login,
    :user_name => ENV['EMAIL_USER_ID'],
    :password => ENV['EMAIL_PASSWORD']
  }

{% endhighlight %}


<br>
### Testing the app

To run the app, cd into the root of the project and issue this command:

{% highlight terminal %}

$ bin/dev

{% endhighlight %} 

Now navigate to <span class="badge">localhost:3000</span>. Create an account by 
clicking on the <span class="badge">sign up</span> link. This should send you an email containing the confirmation instructions.
You will also get reset password emails whenever you reset your password by clicking on <span class="badge">forgot your password</span> link.


<br>
### Additional Information

#### - Redirecting users

1. Redirecting After The User Signs Up (Confirmation Pending)

If you want to redirect the user to a specific url after signing up, override the 
<span class="badge">after_inactive_sign_up_path_for</span> in the <span class="badge">registrations_controller</span>.

 <br>
 i). Create a new <span class="badge">registrations_controller.rb</span> in <span class="badge">app/controllers</span> directory.

{% highlight ruby %}
# app/controllers/registrations_controller.rb

class RegistrationsController < ::Devise::RegistrationsController
  layout false
  # the rest is inherited, so it should work
  
  private
  def after_inactive_sign_up_path_for(resource_or_scope)
    if resource_or_scope == :user || resource_or_scope.class == User || resource_or_scope == User
      your_pending_registrations_path
    #elsif resource_or_scope == :admin || resource_or_scope.class == AdminUser  || resource_or_scope == AdminUser
    #  admin_root_path
    else
      super
    end
  end
  
end

{% endhighlight %} 

 <br>
 ii). Override Devise default behaviour in the routes in <span class="badge">config/routes.rb</span>

{% highlight ruby %} 
# config/routes.rb
 devise_for :users, controllers: { registrations: 'registrations' }
 
{% endhighlight %}  


<br>
2. Redirecting From The Confirmation Email

You may want to redirect the user to a specific url after they clicked the link 
in the confirmation email. To do that, just override the 
<span class="badge">after_confirmation_path_for</span> in the <span class="badge">confirmations_controller</span>.

 <br>
 i). Create a new <span class="badge">confirmations_controller.rb</span> in <span class="badge">app/controllers</span> directory.

{% highlight ruby %}
# app/controllers/confirmations_controller.rb

class ConfirmationsController < Devise::ConfirmationsController
  private
  def after_confirmation_path_for(resource_name, resource)
    sign_in(resource) # In case you want to sign in the user
    # some_other_new_after_confirmation_path
  end
end
{% endhighlight %} 

 <br>
 ii). Override Devise default behaviour in the routes in <span class="badge">config/routes.rb</span>

{% highlight ruby %} 
# config/routes.rb
 devise_for :users, controllers: { confirmations: 'confirmations' }
{% endhighlight %}  
 
 
<br>
*Thanks for reading, see you in the next one!*
