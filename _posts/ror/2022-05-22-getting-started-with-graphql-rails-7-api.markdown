---
layout: post
title:  "Getting Started With graphql Rails 7 API"
author: Denis Kobare
date:   2022-05-22 14:10:00 +0300
img: /assets/img/svg/graphql.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby/Rails/graphql
permalink: "category/:categories/ruby-on-rails/tutorials/:year:month/:title"
---

### Prerequisites

1. Ubuntu 18.04
2. Ruby 3.1.0
3. Rails 7.0.2.4
4. MySQL

*NB: You're at liberty to experiment with other versions and databases.*

<br>
### Create the Rails API Project

1 . Create a new Rails 7 API project

{% highlight console %}

$ rails new your_app_name -d mysql --api
 
{% endhighlight %}  


<br>
2 . Configure session store

Open config/application.rb and add these three lines

{% highlight ruby %}

module Yourappname
  class Application < Rails::Application
    # ...    
    config.session_store :cookie_store, key: '_your_app_name_session'
    config.middleware.use ActionDispatch::Cookies
    config.middleware.use config.session_store, config.session_options    
  end
end
 
{% endhighlight %}  


<br>
3 . Create the Database

{% highlight console %}

$ rake db:create
 
{% endhighlight %}  


<br>
### Configure CORS
Configure CORS to allow frontend to make requests

1 . Uncomment the rack-cors gem in the Gemfile

{% highlight ruby %}
# Gemfile
...
gem 'rack-cors'

{% endhighlight %}  

<br>
2 . Run bundle install

{% highlight console %}

$ bundle install

{% endhighlight %}  

<br>
3 . Uncomment the following code in config/initializers/cors.rb file 

{% highlight ruby %}
# config/initializers/cors.rb

Rails.application.cinfig.middleware.insert_before 0, Rack::Cors do
  allow do
    origins '*' # Allow all hosts to make requests to the API
    
    resource '*'
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end

{% endhighlight %}  


<br>
### Install Graphql

1 . Add graphql in the Gemfile

{% highlight ruby %}
# Gemfile
# ...
gem 'graphql'

{% endhighlight %} 


<br>
2 . Add graphiql-rails within the development dependencies in the Gemfile

Graphiql is the graphql UI that enables interaction with the API endpoints via the browser.

{% highlight ruby %}
# Gemfile

# ...

group :development do
  # ...
  gem 'graphiql-rails'  
end

{% endhighlight %} 


<br>
3 . Add sprockets

An API cannot render any page in the browser. Therefore, to get the GraphiQL editor to work, you must add sprockets to config/application.rb

{% highlight ruby %}
# config/application.rb

# ...

require "sprockets/railtie"

#module Yourappname
#  class Application < Rails::Application
#    # ...    
#    config.session_store :cookie_store, key: '_your_app_name_session'
#    config.middleware.use ActionDispatch::Cookies
#    config.middleware.use config.session_store, config.session_options    
#  end
#end

{% endhighlight %} 


<br>
4 . Install gems

{% highlight terminal %}

$ bundle install

{% endhighlight %} 


<br>
5 . Install graphql boilerplate code

{% highlight terminal %}

$ rails generate graphql:install

{% endhighlight %} 


<br>
6 . Create a config directory in app/assets and create a manifest.js file in it

The manifest.js file specifies additional assets to be compiled and made available to the browser.

The -p and -f flags will instruct the commands to skip creating the directory and the file respectively if they already exist.

{% highlight terminal %}

$ mkdir -p app/assets/config && test -f manifest.js || touch app/assets/config/manifest.js

{% endhighlight %} 


<br>
7 . Open app/config/manifest.js and add this

{% highlight terminal %}

//= link graphiql/rails/application.css
//= link graphiql/rails/application.js

{% endhighlight %} 


<br>
8 . Mount the GraphiQL engine in the development environment in the routes:

This enables access to the GraphiQL UI in the browser

{% highlight ruby %}

Rails.application.routes.draw do

  if Rails.env.development?
    mount GraphiQL::Rails::Engine, at: "/graphiql", graphql_path: "graphql#execute"
  end

  post "/graphql", to: "graphql#execute"
  get "/graphql", to: "graphql#execute"  
  # Define your application routes per the DSL in https://guides.rubyonrails.org/routing.html

  # Defines the root path route ("/")
  # root "articles#index"
end
{% endhighlight %} 


<br>
9 . Test the GraphQL endpoint via the graphiql ui 

Restart your development server, open the browser and navigate to localhost:3000/graphiql
You should see something similar to this:

<img class="zoom-on-hover" srcset="
  {{site.baseurl}}/assets/img/posts/graphql_ui.png 2.5x,
  {{site.baseurl}}/assets/img/posts/graphql_ui.png 5.5x
" alt="missing image">


<br>
10 . Test the API

Clear out the default text in the editor’s left pane and type in the following query:

{% highlight graphql %}

query {
    testField
}

{% endhighlight %} 


Click the Play button to execute the query, you should receive a response as shown below:

<img class="zoom-on-hover" srcset="
  {{site.baseurl}}/assets/img/posts/graphql_hello_world.png 2.5x,
  {{site.baseurl}}/assets/img/posts/graphql_hello_world.png 5.5x
" alt="missing image">

That's it! You have successfully set up a graphql Rails 7 API.


<br>
*Thanks for reading, see you in the next one!*
