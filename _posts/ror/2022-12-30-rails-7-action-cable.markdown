---
layout: post
title:  "Ruby on Rails 7 Action Cable: A Complete Guide with a Practical Example"
author: Denis Kobare
date:   2023-02-11 17:30:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: concepts
technology: Ruby
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---


### Introduction

Ruby on Rails Action Cable is a feature of the Ruby on Rails web framework that 
allows you to add real-time communication to your web applications. With Action 
Cable, you can create real-time connections between your application's client-side 
and server-side, allowing for real-time updates and notifications.

In this section, we'll walk through the process of building a complete real-time 
chat application using Ruby on Rails Action Cable. By the end of this section, 
you'll have a working chat application that allows users to send and receive 
messages in real-time.



<br>
### Step 1: Create a new Ruby on Rails application

The first step is to create a new Ruby on Rails application. To do this, open a 
terminal and run the following command:

{% highlight terminal %}

$ rails new realtime-chat

{% endhighlight  %}

This will create a new Ruby on Rails application in a directory named realtime-chat.


<br>
### Step 2: Generate a Chat Room model and controller

Next, we'll generate a Chat Room model and controller. A Chat Room is where users 
can send and receive messages. To generate the Chat Room model and controller, 
run the following commands:


{% highlight terminal %}

$ rails generate model ChatRoom name:string
$ rails generate controller ChatRooms

{% endhighlight  %}


<br>
### Step 3: Add routes for the Chat Rooms

Now, we'll add routes for the Chat Rooms. Open the config/routes.rb file and add 
the following line:

{% highlight ruby %}
# config/routes.rb

Rails.application.routes.draw do
  resources :chat_rooms
  root to: 'chat_rooms#index'
end

{% endhighlight  %}

This will create RESTful routes for the Chat Rooms and set the root path of the 
application to the index action of the Chat Rooms controller.


<br>
### Step 4: Create the Chat Room view

Next, we'll create the view for the Chat Room. To do this, create a file in 
<span class="badge">app/views/chat_rooms</span> named 
<span class="badge">show.html.erb</span> with the following content:

{% highlight html %}
 <!-- app/views/chat_rooms/show.html.erb -->

<h1><%= @chat_room.name %></h1>
<div id="messages"></div>
<form>
  <input type="text" id="message-input" />
  <input type="submit" value="Send" />
</form>

{% endhighlight  %}


<br>
### Step 5: Generate a channel for the Chat Room

Now, we'll generate a channel for the Chat Room. To do this, run the following command:

{% highlight terminal %}

$ rails generate channel ChatRoom

{% endhighlight  %}

This will generate a channel class in the file <span class="badge">app/channels/chat_room_channel.rb</span>.


<br>
### Step 6: Add broadcast method to the channel

Next, we'll add a broadcast method to the channel. This method will broadcast 
messages to the Chat Room channel. Open the <span class="badge">app/channels/chat_room_channel.rb</span> 
file and add the following code:
 
{% highlight ruby %}
# app/channels/chat_room_channel.rb

class ChatRoomChannel < ApplicationCable::Channel
  def subscribed
    stream_from "chat_room_#{params[:id]}"
  end

  def unsubscribed
    # Any cleanup needed when channel is unsubscribed
  end

  def receive(data)
    ActionCable.server.broadcast "chat_room_#{params[:id]}", message: data['message']
  end
end

{% endhighlight  %}


<br>
### Step 7: Create a JavaScript file for the Chat Room

Next, we'll create a JavaScript file for the Chat Room that will handle sending 
and receiving messages using Action Cable. Create a file named 
<span class="badge">chat_rooms.js</span> in <span class="badge">app/assets/javascripts</span> with the following content:

{% highlight javascript %}
# app/assets/javascripts/chat_rooms.js

$(document).on('turbolinks:load', function() {
  App.chat_room = App.cable.subscriptions.create({ channel: "ChatRoomChannel", id: $("[data-chat-room-id]").data("chat-room-id") }, {
    connected: function() {},
    disconnected: function() {},
    received: function(data) {
      return $("#messages").append(data.message);
    },
    send_message: function(message, chat_room_id) {
      return this.perform('receive', {
        message: message,
        chat_room_id: chat_room_id
      });
    }
  });

  $('#new_message').submit(function(e) {
    e.preventDefault();
    var message = $('#message-input').val();
    App.chat_room.send_message(message, $("[data-chat-room-id]").data("chat-room-id"));
    $('#message-input').val('');
  });
});

{% endhighlight  %}


<br>
### Step 8: Add a form to the Chat Room view

Next, we'll add a form to the Chat Room view that will allow users to send messages. 
Open the <span class="badge">app/views/chat_rooms/show.html.erb</span> file and 
modify the form as follows:


{% highlight html %}

<!-- app/views/chat_rooms/show.html.erb -->

<h1><%= @chat_room.name %></h1>
<div id="messages"></div>
<form id="new_message">
  <input type="text" id="message-input" />
  <input type="submit" value="Send" />
</form>

{% endhighlight  %}


<br>
### Step 9: Add the chat_room_id to the Chat Room view

Finally, we'll add the chat_room_id to the Chat Room view so that it can be used 
in the JavaScript file. Open the <span class="badge">app/views/chat_rooms/show.html.erb</span> 
file and add the following line:


{% highlight ruby %}
<!-- app/views/chat_rooms/show.html.erb -->

<%= content_tag :div, nil, data: { chat_room_id: @chat_room.id } %>

{% endhighlight  %}


<br>
### Step 10: Start the Ruby on Rails server

Now, we're ready to start the Ruby on Rails server and see our real-time chat 
application in action. To start the server, run the following command:


{% highlight terminal %}

$ rails server

{% endhighlight  %}


Open a web browser and navigate to http://localhost:3000 to see the chat application. 
You can open multiple browser tabs and send messages between them to see the real-time updates in action.

In this guide, we've walked through the process of building a complete real-time 
chat application using Ruby on Rails Action Cable. By leveraging the power of 
Action Cable, you can easily add real-time communication to your Ruby on Rails web applications.



<br>
*Thanks for reading, see you in the next one!*
