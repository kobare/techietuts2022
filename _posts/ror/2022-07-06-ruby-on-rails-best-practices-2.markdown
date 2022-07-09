---
layout: post
title:  "Ruby On Rails Best Practices #2"
author: Denis Kobare
date:   2023-07-06 16:20:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: concepts
technology: Ruby/Rails
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---

### Definition

Copying a file on a remote server can be achieved using scp via the terminal. This may come in handy if for example you frequently need to download database files from a production server for backups.
This section will explain how to create some sort of wrapper for that scp task.


<br>
### Create a new Rails 7 project
1 . Create a new Rails 7 project by following [these instructions](/category/frameworks/ruby-on-rails/concepts/202204/creating-a-rails-7-app){:target="_blank"}



<br>
2 . create the home page

{% highlight console %}

$ rails g controller home index

{% endhighlight %}  


<br>
3 . Make home the root route by adding this to the routes.rb file.

{% highlight ruby %}
# config/routes.rb

root "home#index"

# ...

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



*Thanks for reading, see you in the next one!*he creation of
