---
layout: post
title:  "Rails 7 ActionCable"
author: Denis Kobare
date:   2024-01-28 05:45:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: concepts
technology: Ruby
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---


### Definition

Generate a room channel containing speak action
- rails g channel room speak

Two standard callbacks are created in the room channel created.
subscribe and unsubscribe


Enable Action Cable

1. Mount action cable server via the routes file

- mount ActionCable.server => '/cable'

2. Turn on ActionCable on the client side. Add this to cabble.coffee file

- @App ||= {}
- App.cable = ActionCable.createConsumer()



<br>
### Application Example

Suppose we have the orders table and we want to store the status attribute of an 
order object as either completed, pending or cancelled. Assuming there's a status 
column of type integer in the table, having values 0, 1 and 2: 


<br>
#### Defining an enum for the status attribute

{% highlight ruby %}

# app/models/order.rb
class Order < ApplicationRecord
  enum :status, { completed: 0, pending: 1, cancelled: 2 }
end

{% endhighlight  %}


<br>
#### Working with scopes

We can filter a query the regular way or by using the enum helper:

{% highlight ruby %}

@orders = Order.all

@orders.where(status: "pending")

# or

@orders.pending

{% endhighlight  %}


<br>
#### Checking for equality

We can check if an order is "completed" by using <span class="badge">@order.status == "completed"</span> or the enum helper:

{% highlight ruby %}

@order = Order.find(params[:id])

@order.completed? # Returns true if @order.status == "completed"

{% endhighlight  %}


<br>
#### Updating the enum

We can update the enum using the enum names or Rails helpers:
 
{% highlight ruby %}

@order = Order.find(params[:id])

@order.update(status: :cancelled)

 # or
 
@order.cancelled! 

{% endhighlight  %}


<br>
#### Enum options

1 . Using the <span class="badge">prefix</span> or <span class="badge">suffix</span> options to make it more intuitive:
{% highlight ruby %}


class Order < ApplicationRecord
  enum :status, { completed: 0, pending: 1, cancelled: 2 }, prefix: true
end


@orders = Order.all

@orders.status_pending

{% endhighlight  %}


<br>
2 . Disabling scope helper:

The scopes for status won’t be created as we have provided <span class="badge">scopes: false</span>:

{% highlight ruby %}

# app/models/order.rb
class Order < ApplicationRecord
  enum :status, { completed: 0, pending: 1, cancelled: 2 }, prefix: true, scopes: false
end

{% endhighlight  %}


<br>
3 . Opting for array instead of hash:

You can also use an array to define the enum but it's not a good idea, 
because changing the order of the enum values will break the mapping.

{% highlight ruby %}

# app/models/order.rb
class Order < ApplicationRecord
  enum :status, [:completed, :pending, :cancelled], prefix: true
end

{% endhighlight  %}


<br>
*Thanks for reading, see you in the next one!*
