---
layout: post
title:  "Customizing Devise Views"
author: Denis Kobare
date:   2022-08-01 11:45:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby/Rails/Devise
permalink: "category/:categories/ruby-on-rails/tutorials/:year:month/:title"
---

### Introduction

Devise works really great right out of the box but doesn't look good. The pages are
plain so you'd definitely want to replace them with your custom views. It's pretty straight
forward as there's not that much of tinkering needed to get them working. Let's see!

Click [here](https://devise-email-example.herokuapp.com){:target="_blank"} to see the final app.


<br>
### Prerequisites

1. Ruby 3.2.0
2. Rails 7.0.3


<br>
### Part A: Creating a Rails 7 Project 

Create a new rails project called <span class="badge">devise_email</span> by following this document: [Creating A Rails 7 App: With esbuild, bootstrap and jquery](/category/frameworks/ruby-on-rails/tutorials/202204/creating-a-rails-7-app){:target="_blank"}.


<br>
### Part B: Installing Devise

1 . Add <span class="badge">devise</span> gem in the <span class="badge">Gemfile</span> file.

{% highlight ruby %}
# Gemfile

gem 'devise'
  
{% endhighlight %}


<br>
2 . Bundle install

{% highlight terminal %}

$ bundle install
  
{% endhighlight %}


<br>
3 . Setup devise

{% highlight terminal %}

$ rails g devise:install

{% endhighlight %}


<br>
4 . Generate <span class="badge">user</span> model

{% highlight terminal %}

$ rails g devise user

{% endhighlight %}


<br>
### Part C: Creating Custom Views

1 . Generate devise views

{% highlight terminal %}

$ rails generate devise:views

{% endhighlight %}


<br>
2 . Rename <span class="badge">devise</span> folder to <span class="badge">users</span> in <span class="badge">app/views</span>.


<br>
3 . Turn scoped views on in <span class="badge">config/initializers/devise.rb</span>

Devise will automatically check whether the custom view exists and if so, serve that up.

{% highlight ruby %}
# config/initializers/devise.rb
#...

  config.scoped_views = true

{% endhighlight %}


<br>
4 . Include these external stylesheets in the head tag in <span class="badge">views/layouts/application.html.erb</span>

These contain styling and icons for the custom pages that we will be creating in the next step.

{% highlight html %}
<!-- views/layouts/application.html.erb -->

<head>
<!-- ... -->

<link rel="stylesheet" href="https://kobare.github.io/custom_devise_styles/style.css">
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css">    
</head>
{% endhighlight %}
 

<br>
5 . Style up the <span class="badge">Sign In</span> page.
<img class="zoom-on-hover" srcset="
  {{site.baseurl}}/assets/img/posts/devise_sign_in.png 2.5x,
  {{site.baseurl}}/assets/img/posts/devise_sign_in.png 5.5x
" alt="missing image">

Open <span class="badge">views/users/sessions/new.html.erb</span> and replace the code with this code.

<b>Important:</b> We are using a raw form rather than the form helper. Its easier to style the form this way. A form helper
generates the attributes: <span class="badge">id="new_user" action="/users/sign_in" 
accept-charset="UTF-8" method="post"</span>, <span class="badge">input tag names & id</span>, <span class="badge">link urls</span>
and <span class="badge">form authenticity token</span>. Hence we need to add those manually for all the custom pages.

{% highlight html %}
<!-- views/users/sessions/new.html.erb -->

<section class="ftco-section">
 <div class="container">
  <div class="row justify-content-center">
   <div class="col-md-6 text-center mb-5">
    <h2 class="heading-section"><!-- LOGO --></h2>
   </div>
  </div>
  <div class="row justify-content-center">
   <div class="col-md-12 col-lg-10">
    <div class="wrap d-md-flex">
     <div class="text-wrap p-4 p-lg-5 text-center d-flex align-items-center order-md-last">
      <div class="text w-100">
       <%= render "devise/shared/error_messages", resource: resource %>
       <h2>Welcome!</h2>
       <p>Don't have an account?</p>
       <a href="/users/sign_up" class="btn btn-white btn-outline-white">Sign Up</a>
      </div>
     </div>
     <div class="login-wrap p-4 p-lg-5">
      <div class="d-flex">
       <div class="w-100">
        <h3 class="mb-4">Sign In</h3>
       </div>
       <div class="w-100">
        <p class="social-media d-flex justify-content-end">
         <a href="#" class="social-icon d-flex align-items-center justify-content-center"><span class="fa fa-facebook"></span></a>
         <a href="#" class="social-icon d-flex align-items-center justify-content-center"><span class="fa fa-twitter"></span></a>
        </p>
       </div>
      </div>
      <form id="new_user" action="/users/sign_in" accept-charset="UTF-8" method="post" class="signin-form">
       <div class="form-group mb-3">
        <label class="label" for="name">Email</label>
        <input type="text" class="form-control" placeholder="Email" name="user[email]" id="user_email" required>
       </div>
       <div class="form-group mb-3">
        <label class="label" for="password">Password</label>
        <input type="password" class="form-control" placeholder="Password" name="user[password]" id="user_password" required>
       </div>
       <div class="form-group">
        <button type="submit" class="form-control btn btn-primary submit px-3">Sign In</button>
       </div>
       <div class="form-group d-md-flex">
        <div class="w-50 text-left">
         <label class="checkbox-wrap checkbox-primary mb-0">Remember Me
          <input type="checkbox" checked>
          <span class="checkmark"></span>
         </label>
        </div>
        <div class="w-50 text-md-right">
         <a href="/users/password/new">Forgot Password</a>
        </div>
       </div>
       <%= hidden_field_tag :authenticity_token, form_authenticity_token %>
      </form>
     </div>
    </div>
   </div>
  </div>
 </div>
</section>

{% endhighlight %}


<br>
6 . Style up the <span class="badge">Sign Up</span> page.

<img class="zoom-on-hover" srcset="
  {{site.baseurl}}/assets/img/posts/devise_sign_up.png 2.5x,
  {{site.baseurl}}/assets/img/posts/devise_sign_up.png 5.5x
" alt="missing image">

Open <span class="badge">views/users/registrations/new.html.erb</span> and replace the code with this code.


{% highlight html %}
<!-- views/users/registrations/new.html.erb -->

<section class="ftco-section">
 <div class="container">
  <div class="row justify-content-center">
   <div class="col-md-6 text-center mb-5">
    <h2 class="heading-section"><!-- LOGO --></h2>
   </div>
  </div>
  <div class="row justify-content-center">
   <div class="col-md-12 col-lg-10">
    <div class="wrap d-md-flex">
     <div class="text-wrap p-4 p-lg-5 text-center d-flex align-items-center order-md-last">
      <div class="text w-100">
       <%= render "devise/shared/error_messages", resource: resource %>
       <h2>Welcome!</h2>
       <p>Already a member?</p>
       <a href="/users/sign_in" class="btn btn-white btn-outline-white">Sign In</a>
      </div>
     </div>
     <div class="login-wrap p-4 p-lg-5">
      <div class="d-flex">
       <div class="w-100">
        <h3 class="mb-4">Sign Up</h3>
       </div>
       <div class="w-100">
        <p class="social-media d-flex justify-content-end">
         <a href="#" class="social-icon d-flex align-items-center justify-content-center"><span class="fa fa-facebook"></span></a>
         <a href="#" class="social-icon d-flex align-items-center justify-content-center"><span class="fa fa-twitter"></span></a>
        </p>
       </div>
      </div>
      <form id="new_user" action="/users" accept-charset="UTF-8" method="post" class="signin-form">
       <div class="form-group mb-3">
        <label class="label" for="name">Email</label>
        <input type="text" class="form-control" placeholder="Email" name="user[email]" id="user_email" required>
       </div>
       <div class="form-group mb-3">
        <label class="label" for="password">Password</label>
         <% if @minimum_password_length %>
          <em>(<%= @minimum_password_length %> characters minimum)</em>
         <% end %><br>
        <input type="password" class="form-control" placeholder="Password" name="user[password]" id="user_password" required>
        <label class="label" for="password">Password confirmation</label>
        <input type="password" class="form-control" placeholder="Password" name="user[password_confirmation]" id="user_password_confirmation" required>        
       </div>
       <div class="form-group">
        <button type="submit" class="form-control btn btn-primary submit px-3">Sign Up</button>
       </div>
       <div class="form-group d-md-flex">
        <div class="w-50 text-left">
         <label class="checkbox-wrap checkbox-primary mb-0">Remember Me
          <input type="checkbox" checked>
          <span class="checkmark"></span>
         </label>
        </div>
        <div class="w-50 text-md-right">
         <a href="/users/confirmation/new">Resend confirmation link?</a>
        </div>
       </div>
       <%= hidden_field_tag :authenticity_token, form_authenticity_token %>
      </form>
     </div>
    </div>
   </div>
  </div>
 </div>
</section>

{% endhighlight %}
 

<br>
7 . Style up the <span class="badge">Forgot Password</span> page.

<img class="zoom-on-hover" srcset="
  {{site.baseurl}}/assets/img/posts/devise_forgot_password.png 2.5x,
  {{site.baseurl}}/assets/img/posts/devise_forgot_password.png 5.5x
" alt="missing image">

Open <span class="badge">views/users/password/new.html.erb</span> and replace the code with this code.


{% highlight html %}
<!-- views/users/password/new.html.erb -->

<section class="ftco-section">
 <div class="container">
  <div class="row justify-content-center">
   <div class="col-md-6 text-center mb-5">
    <h2 class="heading-section"><!-- LOGO --></h2>
   </div>
  </div>
  <div class="row justify-content-center">
   <div class="col-md-12 col-lg-10">
    <div class="wrap d-md-flex">
     <div class="text-wrap p-4 p-lg-5 text-center d-flex align-items-center order-md-last">
      <div class="text w-100">
       <%= render "devise/shared/error_messages", resource: resource %>
       <h2>Welcome!</h2>
       <p> Didn't receive confirmation link?</p>
       <a href="/users/confirmation/new" class="btn btn-white btn-outline-white">Resend Link</a>
      </div>
     </div>
     <div class="login-wrap p-4 p-lg-5">
      <div class="d-flex">
       <div class="w-100">
        <h3 class="mb-4">Forgot&nbsp;password?</h3>
       </div>
       <div class="w-100">
        <p class="social-media d-flex justify-content-end">
         <a href="#" class="social-icon d-flex align-items-center justify-content-center"><span class="fa fa-facebook"></span></a>
         <a href="#" class="social-icon d-flex align-items-center justify-content-center"><span class="fa fa-twitter"></span></a>
        </p>
       </div>
      </div>
      <form id="new_user" action="/users/password" accept-charset="UTF-8" method="post" class="signin-form">
       <div class="form-group mb-3">
        <label class="label" for="name">Email</label>
        <input type="text" class="form-control" placeholder="Email" name="user[email]" id="user_email" required>
       </div>
       <div class="form-group">
        <button type="submit" class="form-control btn btn-primary submit px-3">Send Password Reset Link</button>
       </div>
       <div class="form-group d-md-flex">
        <div class="w-50 text-left">
         <a href="/users/sign_in">Sign In</a>        
        </div>
        <div class="w-50 text-md-right">
         <a href="/users/sign_up">Sign Up</a>
        </div>
       </div>
       <%= hidden_field_tag :authenticity_token, form_authenticity_token %>
      </form>
     </div>
    </div>
   </div>
  </div>
 </div>
</section>

{% endhighlight %}


<br>
8 . Finally, Style up the <span class="badge">Email Confirmation</span> page.

<img class="zoom-on-hover" srcset="
  {{site.baseurl}}/assets/img/posts/devise_confirmation.png 2.5x,
  {{site.baseurl}}/assets/img/posts/devise_confirmation.png 5.5x
" alt="missing image">

Open <span class="badge">views/users/confirmation/new.html.erb</span> and replace the code with this code.


{% highlight html %}
<!-- views/users/confirmation/new.html.erb -->

<section class="ftco-section">
 <div class="container">
  <div class="row justify-content-center">
   <div class="col-md-6 text-center mb-5">
    <h2 class="heading-section"><!-- LOGO --></h2>
   </div>
  </div>
  <div class="row justify-content-center">
   <div class="col-md-12 col-lg-10">
    <div class="wrap d-md-flex">
     <div class="text-wrap p-4 p-lg-5 text-center d-flex align-items-center order-md-last">
      <div class="text w-100">
       <%= render "devise/shared/error_messages", resource: resource %>
       <h2>Welcome!</h2>
       <p>Forgot your password?</p>
       <a href="/users/password/new" class="btn btn-white btn-outline-white">Send Password Reset Link</a>
      </div>
     </div>
     <div class="login-wrap p-4 p-lg-5">
      <div class="d-flex">
       <div class="w-100">
        <h3 class="mb-4">Resend Confirmation Link</h3>
       </div>
      </div>
      <form id="new_user" action="/users/confirmation" accept-charset="UTF-8" method="post" class="signin-form">
       <div class="form-group mb-3">
        <label class="label" for="name">Email</label>
        <input type="text" class="form-control" placeholder="Email" name="user[email]" id="user_email" required>
       </div>
       <div class="form-group">
        <button type="submit" class="form-control btn btn-primary submit px-3">Resend Confirmation Link</button>
       </div>
       <div class="form-group d-md-flex">
        <div class="w-50 text-left">
         <a href="/users/sign_in">Sign In</a>        
        </div>
        <div class="w-50 text-md-right">
         <a href="/users/sign_up">Sign Up</a>
        </div>
       </div>
       <%= hidden_field_tag :authenticity_token, form_authenticity_token %>
      </form>
     </div>
    </div>
   </div>
  </div>
 </div>
</section>

{% endhighlight %}

 
<br>
*Thanks for reading, see you in the next one!*
