---
layout: post
title:  "Getting Started with Jekyll: Your Step-by-Step Guide"
author: Denis Kobare
date:   2024-08-01 15:30:00 +0300
img: /assets/img/svg/jekyll.svg
categories: frameworks
sub_category: jekyll
type: tutorials
technology: Ruby
permalink: "category/:categories/jekyll/tutorials/:year:month/:title"
---


### Introduction

Jekyll is a popular static site generator that helps you create simple, fast, 
and efficient websites. It's widely used for blogs, personal websites, and even 
documentation sites. Jekyll uses Markdown, a lightweight markup language, to 
create content, and then it generates static HTML files that can be hosted on 
any web server.

In this guide, we'll walk you through the basics of setting up Jekyll, including 
installation, creating a new site, and running the development server. By the 
end, you'll have a solid foundation to start building your own Jekyll-powered 
website.



<br>
### Prerequisites

Before we begin, make sure you have the following installed on your system:

- Ruby: Jekyll is built using the Ruby programming language. You can check if 
Ruby is installed by running ruby -v in your terminal. If not, you can download 
it from the official Ruby website.

- RubyGems: RubyGems is the package manager for Ruby. You can check if it's 
installed by running gem -v in your terminal. If not, it usually comes 
pre-installed with Ruby, but you can find installation instructions on the 
RubyGems website.

- Git: Jekyll projects are often managed using Git for version control. You can 
check if Git is installed by running git --version in your terminal. If not, you 
can download it from the official Git website.



<br>
### Step 1: Install Jekyll

To install Jekyll, open your terminal and run the following command:

{% highlight terminal %}

gem install jekyll bundler

{% endhighlight %}


This command installs both Jekyll and Bundler, a tool that helps manage Ruby gem 
dependencies for your projects.



<br>
### Step 2: Create a New Jekyll Site

Once Jekyll is installed, you can create a new site using the following command:

{% highlight terminal %}

jekyll new my-awesome-site

{% endhighlight %}


This command will create a new directory called "my-awesome-site" with the basic 
structure for a Jekyll site.




<br>
### Step 3: Navigate to Your New Site

Navigate to the newly created site directory:

{% highlight terminal %}

cd my-awesome-site

{% endhighlight %}



<br>
### Step 4: Serve Your Site Locally

To preview your site locally, use the following command:

{% highlight terminal %}

bundle exec jekyll serve

{% endhighlight %}


This command will start a development server, and you can view your site by 
opening a web browser and navigating to http://localhost:4000.



<br>
### Step 5: Customize Your Site

Your Jekyll site is now up and running, but it's using the default theme and 
content. Let's make it your own by customizing it.

- Choose a Theme: Jekyll has a wide range of themes you can use to change the 
look and feel of your site. You can find themes on the Jekyll Themes website. 
Once you find a theme you like, follow the theme's instructions to install and 
configure it.

- Edit Content: The main content of your site is located in Markdown files 
(.md) within the _posts and _pages directories. You can edit these files using a 
text editor of your choice. Make sure to follow Markdown formatting rules.

- Configuration: Customize your site by editing the _config.yml file. This file 
contains various settings for your Jekyll site, such as site title, description, 
and more.



<br>
### Conclusion

Congratulations! You've successfully set up Jekyll, created a new site, and 
learned how to run the development server. You're now ready to dive deeper into 
Jekyll, explore its features, and build a fantastic static website. Remember to 
explore Jekyll's documentation to learn more about advanced topics and features. 
Happy coding!



<br>
*Thanks for reading, see you in the next one!*
