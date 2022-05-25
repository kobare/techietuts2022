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

### Definition

Creating a Rails project is as simple as running one line of command on the terminal. While it's easy to do so, it's imperative to configure the basic tools for the project. This section outlines the necessary steps for creating and configuring a new Rails 7 project.
We will use esbuild for JavaScript bundling, bootstrap for CSS and JQuery for JavaScript and SQLite as the database.


<br>
### Dependencies
- rails 7.0.0 or higher (Rails 7.0.2.3 was used at the time of writing this document)
- npm version 8.6.0 (or higher, nmp 8.6.0 was used at the time of writing this document)
- yarn



<br>
### Create the project
1 . Open the terminal, cd into your projects directory and run this  command:

{% highlight terminal %}

$ rails new my_app -j esbuild --css bootstrap

{% endhighlight %} 

The -j directive is used to define the JavaScript bundler while the - -css directive defines the CSS library.
<br>NB: The bootstrap version installed will be the latest version.


<br>
2 . cd into my_app and add jquery and jquery-ui

{% highlight ruby %}

$ yarn add jquery jquery-ui

{% endhighlight %}  


<br>
3 . Import jquery and jquery-ui in app/javascript/application.js

Normally, this is how we would import jquery and jquery-ui in application.js.
{% highlight javascript %}
// app/javascript/application.js
// ...

import jquery from 'jquery';
window.jQuery = jquery;
window.$ = jquery;
import "jquery-ui"

{% endhighlight %} 

But since JavaScript imports are hoisted; i.e. all imports are loaded before any other code regardless of the order. This creates a problem because JavaScript will try to load jquery-ui before jquery is made available to the window. No problem, here's the fix:


i). In app/javascript, create src sub-directory and a jquery.js file and add this to it:

{% highlight javascript %}
// app/javascript/src/jquery.js

import jquery from 'jquery';
window.jQuery = jquery;
window.$ = jquery;

{% endhighlight %} 

<br>
 ii). In app/javascript/application.js add this:

{% highlight javascript %}
// app/javascript/application.js
// ...

import "./src/jquery"
import "jquery-ui"

{% endhighlight %} 

<br>
### Run the app
To run the app, do this:

{% highlight terminal %}

$ bin/dev

{% endhighlight %} 

If you wish to change the default port, open the Procfile.dev file located in the root of the project and change the port number.

{% highlight text %}
// /Procfile.dev
// ...

web: bin/rails server -p 3000
js: yarn build --watch
css: yarn build:css --watch

{% endhighlight %} 



<br>
We have covered the necessary configurations for the project. If you run the app, you should now be able to use jquery and bootstrap to build the project.



*Thanks for reading, see you in the next one!*
