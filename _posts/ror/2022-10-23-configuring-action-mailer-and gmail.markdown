---
layout: post
title:  "Configuring ActionMailer and Gmail"
author: Denis Kobare
date:   2022-10-23 07:30:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby/Rails/Devise
permalink: "category/:categories/ruby-on-rails/tutorials/:year:month/:title"
---

### Introduction

When ActionMailer and Gmail settings are properly configured, we can easily send emails from 
controller actions by generating and setting up mailers.

The set up is really easy. Let's have a look.

<br>
### Part A: Setting Up Gmail App Passwords

<span class="badge">ActionMailer</span> will use your gmail account to send mail, but you can not use your normal gmail password with external applications. Gmail has a nifty feature called [App passwords](https://myaccount.google.com/apppasswords){:target="_blank"} that lets you sign in to your Google Account from apps on devices that don't support 2-Step Verification. Click [here](https://myaccount.google.com/apppasswords){:target="_blank"} to set up App passwords. On the page, select 'Mail' under the 'select app' tab, then enter a custom name under the 'select device' tab.

Click on the 'Generate' button to generate the password. Just like your normal password, this app password grants complete access to your Google Account. Copy 16-character password shown, you will need it for the next step.


<br>
### Part B: Setting Up Environment Variables For the Email Credentials

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
### Part C: Configure <span class="badge">ActionMailer</span>

<br>
1 . Configure <span class="badge">ActionMailer</span> for the development 
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

That's all!
 
<br>
*Thanks for reading, see you in the next one!*
