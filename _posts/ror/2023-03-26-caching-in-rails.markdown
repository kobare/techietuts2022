---
layout: post
title:  "Caching in Rails"
author: Denis Kobare
date:   2023-03-26 11:40:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: concepts
technology: Ruby
permalink: "category/:categories/ruby-on-rails/concepts/:year:month/:title"
---


### Introduction

Rails provides several types of caching to help improve the performance of your 
application. The three most commonly used types of caching are fragment caching, 
action caching, and HTTP caching. Let's take a closer look at each type of caching 
and when to use it.



<br>
### Page Caching

Page caching is the simplest form of caching available in Rails 7. It involves 
saving the entire HTML response of a page to a file and serving the same file for 
subsequent requests. This type of caching is useful for pages that don't have any 
dynamic content, such as homepages, about us pages, or other static pages.

To enable page caching, you can use the caches_page method in your controller, like this:

<br>
{% highlight ruby %}

class HomeController < ApplicationController
  caches_page :index

  def index
    # code to render the page
  end
end

{% endhighlight %}


<br>
In this example, the <span class="badge">caches_page</span> method caches the 
HTML response for the <span class="badge">index</span> action of the 
<span class="badge">HomeController</span> class. Whenever a user requests 
the <span class="badge">index</span> page, Rails will serve the cached HTML 
response instead of rendering the page again.


<br>
### Fragment Caching:

Fragment caching is used to cache parts of a view that are expensive to generate. 
For example, if you have a page that includes a list of articles and each article 
has many comments, rendering the page may take a long time if you have to load 
all the comments for each article on every request. To speed up the rendering process, 
you can use fragment caching to cache the comments for each article separately.

To use fragment caching, wrap the expensive code in a cache block like this:

<br>
{% highlight erb %}

<% cache @article.comments do %>
  <%= render @article.comments %>
<% end %>

{% endhighlight %}


<br>
This will cache the output of the expensive code and use it on subsequent requests, 
avoiding the need to regenerate it each time. In this example, the comments for 
each article are cached separately, so if a new comment is added to one of the articles, 
only the cached fragment for that article will be invalidated.



<br>
### Action Caching:

Action Caching works like Page Caching except the incoming web request hits the 
Rails stack so that <span class="badge">before</span> filters can be run on it 
before the cache is served.

This ensures that any actions needing to take place before the cached copy is hit 
can be performed. Therefore increasing performance while taking into account the 
functionality required to access parts of your application.

Suppose you have a before filter that checks whether the user is authorized to 
view the article. If the user is authorized, the cached copy of the article is 
served, and if the user is not authorized, the before filter redirects the user 
to the login page.

Here's an example of how you can use Action Caching in your Rails 7 application:


<br>
{% highlight ruby %}

class ArticlesController < ApplicationController
  before_action :authenticate_user!, except: [:index, :show]

  def show
    @article = Article.find(params[:id])
    # ... other code to render the article page ...
    cache_action unless current_user.admin?
  end

  private

  def cache_action
    response.headers['Cache-Control'] = 'public, max-age=300'
    expires_in 5.minutes
  end
end

{% endhighlight %}


<br>
In this example, we have a <span class="badge">before_action :authenticate_user!</span>, 
which ensures that only authenticated users can view the article. In the show 
action, we use cache_action to cache the output of the action for 5 minutes, 
but only if the current user is not an admin. This ensures that non-admin users 
get the cached copy of the article, and the before_filter 
<span class="badge">:authenticate_user!</span> is executed before serving the cached copy.


<br>
### HTTP Caching:

HTTP caching is used to cache responses from the server and avoid unnecessary 
network requests. For example, if you have a page that displays a list of products, 
you can use HTTP caching to allow the client to cache the response and avoid 
downloading unnecessary data.

To use HTTP caching, add the following code to your controller action:

<br>
{% highlight ruby %}

def index
  @products = Product.all
  fresh_when last_modified: @products.maximum(:updated_at), public: true
end

{% endhighlight %}


<br>
This will set the <span class="badge">Last-Modified</span> and 
<span class="badge">ETag</span> headers on the response, allowing the 
client to make conditional requests and avoid downloading unnecessary data. 
In this example, the response will be cached based on the 
<span class="badge">last_modified</span> timestamp of the products, so if a new 
product is added or an existing product is updated, the cached response will be 
invalidated and regenerated.

In addition to these types of caching, Rails also provides other caching mechanisms 
like low-level caching and Russian doll caching. Low-level caching is used to cache 
arbitrary objects, such as database queries or API responses. Russian doll caching 
is used to cache nested fragments of a view, which can be useful when you have 
complex views with many nested elements.



<br>
### Conclusion
When using caching in your Rails application, it's important to consider the 
trade-offs between performance and complexity. Caching can significantly improve 
the performance of your application, but it can also introduce some complexity 
and potential issues, such as cache invalidation and cache consistency. 
Be sure to test your application thoroughly with caching enabled and monitor its 
performance to ensure that it's working as expected.

<br>
*Thanks for reading, see you in the next one!*
