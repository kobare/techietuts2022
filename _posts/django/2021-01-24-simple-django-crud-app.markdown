---
layout: post
title:  "Build a Simple Django Blog App"
author: Denis Kobare
date:   2021-01-24 16:15:00 +0300
img: /assets/img/svg/django.svg
categories: frameworks
sub_category: django
type: tutorials
technology: python/django
tutorial_type: How To
permalink: "category/:categories/django/tutorials/:year:month/:title"
tags:
- python
- django
- tutorial
- crud
---


This tutorial will focus on how to create a standard Django CRUD project.

  <table class="table table-bordered">
    <thead>
      <tr>
        <th colspan="5" style="text-align: center;">This tutrial will cover the following:</th>
      </tr>
    </thead>
    <tbody>
      <tr>
       <td>1. URLs</td>
      </tr>
      <tr>
       <td>2. Templates</td>
      </tr>
      <tr>
       <td>3. Views</td>
      </tr>
      <tr>
       <td>4. Models</td>
      </tr>
      <tr>
       <td>5. Django Admin</td>
      </tr>
      
    </tbody>
  </table><br>


If you haven't installed and configured virtual environment and the virtualenvwrapper on your machine, I have a tutorial that will show you how to. Click [here]({{site.baseurl}}/category/programming/python/setup-and-configurations/202101/how-to-install-python-3-and-set-up-a-local-programming-environment-on-ubuntu-20.04){:target="_blank"} to read it.

### 1. Create a Virtual Environment 
<section class="terminal-container terminal-fixed-top">
cd into your projects directory and pass the following command to create and activate a new virtual environment named my_website
<br><br>
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
<p class="console">mkvirtualenv my_website</p>
</div>
</section>

<br><br>
 

### 2. Install the latest version of Django
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
<p class="console">pip install django</p>
 <h6 class="hashed">Verify and check the version installed</h6>
<p class="console">django-admin --version </p>
</div>
</section><br><br><br>


### 3. Create the Django Project
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
 <p class="console">django-admin startproject my_website</p>
 <h6 class="hashed"># cd into the project: </h6>
 <p class="console">cd my_website</p>
 <h6 class="hashed">Run the development server to make sure everything is ok so far</h6>
 <p class="console">python manage.py runserver</p>
</div>
</section><br>


Now if you navigate to [http://localhost:8000/](http://localhost:8000/){:target="_blank"} on your browser and you should see something like this:

<img srcset="
  {{site.baseurl}}/assets/img/django_project_page.png 1.5x,
  {{site.baseurl}}/assets/img/django_project_page.png 3.5x
" alt="missing image">
<br><br>

    Now close the development server with Ctrl + C 

### 4. Create a Django App
A django project can have multiple apps in it. For example, you can have an ecommerce app and a blog app within one project. With this approach, Django allows you to manage the apps independently and you can easily move your apps into other projects if need be. 
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
 <p class="console">python manage.py startapp blog</p>
</div>
</section><br><br><br>


Open the settings.py file and specify the allowed hosts as well as add the blog app to installed apps. You should have something similar to this:
<img srcset="
  {{site.baseurl}}/assets/img/blog_app_settings.png 1x,
  {{site.baseurl}}/assets/img/blog_app_settings.png 2x
" alt="missing image">
<br><br>


At this point, the project tree should resemble this:
<img srcset="
  {{site.baseurl}}/assets/img/blog_app_tree.png 1x,
  {{site.baseurl}}/assets/img/blog_app_tree.png 2x
" alt="missing image">
<br><br>


### 6. Create a Templates Directory
If you are familiar with Ruby on Rails, Templates in Django are the equivalent of Views in Rails. Basically, this is where the HTML files go. Open the blog directory and create a directory called templates.


### 7. Create a Blog Sub-Directory
Within the templates directory, create a sub-directory called blog. For every app in your project, you must create a sub-directory within the templates directory with the same name as your app. For instance, if we created another app in our project called ecommerce, we would then create a sub-directory inside templates directory called ecommerce.




That's it! You have successfully set up, configured and tested python.

*See you in the next tuts*

