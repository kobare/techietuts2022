---
layout: post
title:  "Creating A Rails 7 App: With esbuild, bootstrap and jquery"
author: Denis Kobare
date:   2022-04-10 14:27:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby/Rails
permalink: "category/:categories/ruby-on-rails/tutorials/:year:month/:title"
---

### Introduction

Creating a Rails project is as simple as running one line of command on the terminal. While it's easy to do so, it's imperative to configure the basic tools for the project. This section outlines the necessary steps for creating and configuring a new Rails 7 project.
We will use esbuild for JavaScript bundling, bootstrap for CSS and JQuery for JavaScript and SQLite as the database.


<br>
### Prerequisites
- Ruby 3.2.0
- Rails 7.0.0 or higher (Rails 7.0.2.3 was used at the time of writing this document)
- NPM version 8.6.0 (or higher, nmp 8.6.0 was used at the time of writing this document).
You can update npm by running: <span class="badge">sudo npm install -g npm@latest</span> or <span class="badge">sudo npm install -g npm</span>
- Yarn


<br>
### Creating the project
1 . Open the terminal, cd into your projects directory and run this  command:

{% highlight terminal %}

$ rails new my_app -j esbuild --css bootstrap

{% endhighlight %} 

The <span class="badge">-j</span> directive is used to define the JavaScript 
bundler while the <span class="badge">- -css</span> directive defines the CSS library.

If you don't want to use the default <span class="badge">SQLite</span> database, 
you can specify a different database with the <span class="badge">-d</span> directive like so:

{% highlight terminal %}
$ rails new my_app -j esbuild --css bootstrap -d mysql
{% endhighlight %}  

<br>
Even after the command has created the project, it is possible to switch 
databases like so, in root of the project:

{% highlight terminal %}

$ rails db:system:change --to=postgresql 

{% endhighlight %}  

<br>_NB: The bootstrap version installed will be the latest version._


<br>
2 . cd into <span class="badge">my_app</span> and add <span class="badge">jquery</span>, <span class="badge">jquery-ujs</span> and <span class="badge">jquery-ui</span>

{% highlight ruby %}

$ yarn add jquery jquery-ujs jquery-ui

{% endhighlight %}  


<br>
3 . Import <span class="badge">jquery</span>, <span class="badge">jquery-ujs</span> and <span class="badge">jquery-ui</span> in <span class="badge">app/javascript/application.js</span>

Normally, this is how we would import them.

{% highlight javascript %}
// app/javascript/application.js
// ...

import jquery from 'jquery';
window.jQuery = jquery;
window.$ = jquery;
import "jquery-ui"

{% endhighlight %} 

But since JavaScript imports are hoisted; i.e. all imports are loaded before any 
other code regardless of the order, it causes a problem because JavaScript will 
try to load <span class="badge">jquery-ui</span> before <span class="badge">jquery</span> is made available to the window. Let's fix that:


i). In <span class="badge">app/javascript</span>, create <span class="badge">src</span> 
sub-directory and in it create <span class="badge">jquery.js</span> file and add this to it:

{% highlight javascript %}
// app/javascript/src/jquery.js

import jquery from 'jquery';
window.jQuery = jquery;
window.$ = jquery;

{% endhighlight %} 

<br>
 ii). In <span class="badge">app/javascript/application.js</span> add this:

{% highlight javascript %}
// app/javascript/application.js
// ...

import "./src/jquery"
import "jquery-ui"
import "jquery-ujs"

{% endhighlight %} 


<br>
4 . Set up notifications

 i). Add notifications in <span class="badge">app/views/layouts/application.html.erb</span>

{% highlight html %}
<!-- views/layouts/application.html.erb -->
<!-- .... -->  
  <body>
   <% flash.each do |key, value| %>
    <div class="<%= flash_class(key) %>">
     <%= value %>
    </div>
   <% end %>
    <%= yield %>
  </body>

{% endhighlight %} 


 <br>
 ii). Create the <span class="badge">flash_class</span> method in <span class="badge">app/helpers/application_helper.rb</span>

{% highlight ruby %}
# app/helpers/application_helper.rb
# ...

  def flash_class(level)
    case level
      when :notice then "alert alert-info"
      when :success then "alert alert-success"
      when :error then "alert alert-danger"
      when :alert then "alert alert-danger"
    end
  end

{% endhighlight %} 



<br>
5 . Create a navbar in <span class="badge">app/views/layouts/application.html.erb</span>

{% highlight html %}
<!-- views/layouts/application.html.erb -->
<!-- .... -->  

  <body>

   <nav class="navbar navbar-expand-sm bg-dark navbar-dark fixed-top">
    <div class="container-fluid">
     <a class="navbar-brand" href="#">Rails 7 Bootstrap</a>
     <a class="navbar-brand" href="#">Jquery</a>
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
6 . Create The Home Page


 i) Generate the controller and view

{% highlight terminal %}

$ rails g controller home index

{% endhighlight %} 


 <br>
 ii) Open the home page in <span class="badge">app/views/home/index.html.erb</span> and add this code

{% highlight html %}
<!-- app/views/home/index.html.erb --> 

<div class="container mt-3">
  <h3>See if bootstrap is working</h3>
  
  <button type="button" class="btn btn-primary" data-bs-toggle="tooltip" title="Yay! Its' working.">
    Hover over me!
  </button>  
</div>

<div class="container mt-3">
  <h3>See if jquery is working</h3>
  
  <button type="button" class="btn btn-primary" id="jq-test">
    Click me!
  </button>
</div>
   
{% endhighlight %} 


 <br>
 ii) Create <span class="badge">home.js</span> file in <span class="badge">app/javascript/src</span> and add this jquery code.
  
{% highlight js %} 
// javascript/src/home.js

$("#jq-test").click(function(){
  alert("Jquery is working!")
});
 
{% endhighlight %} 

 <br>
 iii) Import <span class="badge">home.js</span> in <span class="badge">app/javascript/application.js</span>

{% highlight js %} 
// app/javascript/src/application.js
// ...

import "./src/home"
 
{% endhighlight %} 
 
  
 <br>
 iv) Set home page as the default route in <span class="badge">config/routes.rb</span>

{% highlight ruby %}
# config/routes.rb
 
  root "home#index"

{% endhighlight %} 

<br>
### Testing the app

To run the app, cd into the root of the project and issue this command:

{% highlight terminal %}

$ bin/dev

{% endhighlight %} 

Now navigate to <span class="badge">localhost:3000</span>. The two buttons should be working if you've made the configurations described here.
If you wish to change the default port, open the <span class="badge">Procfile.dev</span> file located in the root of the project and change the port number.

{% highlight text %}
// /Procfile.dev
// ...

web: bin/rails server -p 3000
js: yarn build --watch
css: yarn build:css --watch

{% endhighlight %} 


<br>
### Something to remember
If you are going to use inline or embedded javascript in your app pages, jquery will not work.

For jquery to work, in <span class="badge">app/views/layouts/application.html.erb</span>, set 
 <span class="badge">defer: false</span> in the javascript_include_tag.
 If the option is set to <span class="badge">true</span>, the browser will throw
 this error <span class="badge">Uncaught ReferenceError: $ is not defined</span>.
 
 _NB: Defer loads javascript last to prevent it from blocking app content from
 loading. Therefore, <span class="badge">defer: false</span> can prevent the app from 
 loading smoothly i.e makes the page appear to freeze halfway through the rendering.
 You're better off using external scripts._


<br>
We have covered the necessary configurations for the project. If you run the app, you should now be able to use jquery and bootstrap to build the project.



*Thanks for reading, see you in the next one!*
