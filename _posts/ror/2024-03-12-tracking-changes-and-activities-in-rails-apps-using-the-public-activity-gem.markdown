---
layout: post
title:  "Tracking Changes and Activities in Rails Apps Using the Public Activity Gem"
author: Denis Kobare
date:   2024-03-12 15:30:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby
permalink: "category/:categories/ruby-on-rails/tutorials/:year:month/:title"
---


### Introduction

The Public Activity gem provides easy-to-use functionality for tracking changes 
and activities in your Rails application. It allows you to track actions such as 
creating, updating, and deleting records, providing insights into the activities 
happening within your application. With Public Activity, you can easily implement 
activity feeds, auditing, and user tracking features.



<br>
### Step 1: Adding the Gem to Your Gemfile

To begin using the Public Activity gem, you need to add it to your Rails 
application's Gemfile. Open your Gemfile and add the following line:

{% highlight ruby %}

gem 'public_activity'

{% endhighlight %}


After adding the gem, run bundle install in your terminal to install the gem 
and its dependencies:

{% highlight terminal %}

$ bundle install

{% endhighlight %}




<br>
### Step 2: Generate Necessary Migrations

Run the following command to generate the necessary migrations for PublicActivity:

{% highlight terminal %}

$ rails generate public_activity:migration

# Then, run this to apply these migrations to your database.
$ rails db:migrate 

# This generates the activities table in your database.
{% endhighlight %}




<br>
### Step 3: Setting Up Tracking in Your Model

In order to track activities for a specific model, you need to include the 
PublicActivity module in that model. Let's assume you want to track activities 
for an Invoice model.

Open your invoice.rb model file located in app/models and add the following code:

{% highlight ruby %}

class Invoice < ApplicationRecord
  include PublicActivity::Model
  tracked owner: Proc.new{ |controller, model| controller.current_user || controller.current_admin_user }
end

{% endhighlight %}


This code snippet adds tracking functionality to the Invoice model. It specifies 
that the owner of the activity is either the current user or the current admin 
user, depending on the controller context.


<br>
### Step 4: Integrating PublicActivity into Your ApplicationController

To enable tracking and access the current user or admin user in your controllers, 
you need to include the PublicActivity::StoreController module in your 
ApplicationController.

Open your application_controller.rb file located in app/controllers and add the 
following line:

{% highlight ruby %}

class ApplicationController < ActionController::Base
  include PublicActivity::StoreController
end

{% endhighlight %}


This line ensures that the PublicActivity gem is available throughout your 
application and can access the necessary controller context.



<br>
### Step 5: Implementing a Helper Method

To retrieve information about who performed a specific action, you can create a 
helper method. This method will help you display the user who performed a 
particular action in your views.

Open your helper.rb file located in app/helpers and add the following code:

{% highlight ruby %}

module ApplicationHelper
  def served_by(record)
    class_name = record.class.name
    activity = PublicActivity::Activity.where(trackable_id: record.id, trackable_type: class_name, key: "#{class_name.underscore}.create").first
    if activity
      return activity.try(:owner).try(:name)
    end
  end
end

{% endhighlight %}


This helper method queries the PublicActivity database to retrieve the activity 
associated with the provided record. It then returns the name of the user who 
performed the action.


<br>
### Step 6: Displaying Activity Information in Your Views

Now that you have set up tracking and defined a helper method, you can display 
activity information in your views. Let's assume you want to display the user 
who served an invoice.

Open your invoices.html.erb file or any other relevant view file, and add the 
following line where you want to display the served by information:

{% highlight html %}

Served by: <%= served_by(@invoice).titleize if served_by(@invoice) %>

{% endhighlight %}

This line calls the served_by helper method passing the @invoice object. It 
displays the name of the user who served the invoice if available.



<br>
### Conclusion

You have successfully installed and integrated the Public Activity gem into your 
existing Rails application. You can now track activities, such as creating 
invoices, and display relevant information about those activities in your views. 
Public Activity provides a powerful way to monitor and analyze user interactions 
within your application.



<br>
*Thanks for reading, see you in the next one!*
