---
layout: post
title:  "Creating a Rails API With Jason Web Token (JWT) Authentication"
author: Denis Kobare
date:   2022-09-22 05:45:00 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby
permalink: "category/:categories/ruby-on-rails/tutorials/:year:month/:title"
---

### Introduction

Token based authentication is an alternative method to session-based authentication. 
The server creates a web token (JWT), encodes, serializes and signs it with 
its own secret key so that when it's tampered with, the server will know and reject it.

Because the JWT created contains all the information about the user, it is sent to the browser,
and so the server does not need to store information about the user.

Let's create a Rails API that authenticates users with JWT.

<br>
### Prerequisites

- postman (desktop version)


<br>
### Create the Project

1 . Create a rails API only project.

{% highlight terminal %}

$ rails new jwt_api --api -d mysql

{% endhighlight %}



<br>
2 . Add JSON Web Token (JWT) and bcrypt gem in <span class="badge">Gemfile</span>.

{% highlight ruby %}
# ...

# Use JSON Web Token (JWT) for token based authentication
gem "jwt"

# Use ActiveModel has_secure_password
gem "bcrypt", "~> 3.1.7"

{% endhighlight %}


<br>
3 . Bundle up

{% highlight terminal %}

$ bundle install

{% endhighlight %}


<br>
4 . Create the database

{% highlight terminal %}

$ rails db:create

{% endhighlight %}


<br>
5 . Create the user model

{% highlight terminal %}

$ rails g model user name:string username:string email:string password_digest:string

$ rails db:migrate

{% endhighlight %}


<br>
6 . Require <span class="badge">securerandom</span> for generating session keys.

{% highlight ruby %}
# app/models/user.rb

class User < ApplicationRecord
  require "securerandom"
 
  has_secure_password
 
  validates :email, presence: true
  validates :password, presence: true
  validates :username, presence: true, uniqueness: true
 
end

{% endhighlight %}


<br>
7 . Generate users controller.
{% highlight terminal %}

$ rails g controller users

{% endhighlight %}


<br>
8 . Add actions in users controller.

{% highlight ruby %}

class UsersController < ApplicationController
  skip_before_action :authenticate_request, only: [:create]
  before_action :set_user, only: [:show, :destroy]
  
  # GET /users
  def index
    @users = User.all
    render json: @users, status: :ok
  end

  
  # GET /users/{username}
  def show
    render json: @user, status: :ok
  end
  
 
  # POST /users
  def create
    @user = User.new(user_params)
    if @user.save
      render json: @user, status: :created
    else
      render json: { errors: @user.errors.full_messages }, 
             status: :unprocessable_entity
    end
  end
  
  
  # PUT /users/{username}
  def update
    unless @user.update(user_params)
      render json: { errors: @user.errors.full_messages },
             status: :unprocessable_entity
    end
  end
  
  
  # DELETE /users/{username}
  def destroy
    @user.destroy
  end
  
  
  private
    def user_params
      params.permit(:username, :name, :email, :password)
    end
  
  
    def set_user
      @user = User.find(params[:id])
    end
    
end

{% endhighlight %}


<br>


<br>
9 . Create JsonWebToken concerns in <span class="badge">app/controllers/concerns</span>.

{% highlight ruby %}
# app/controllers/concerns/json_web_token.rb

require "jwt"
module JsonWebToken
  extend ActiveSupport::Concern
  SECRET_KEY = Rails.application.secret_key_base
  
  
  def jwt_encode(payload, exp = 7.days.from_now)
    payload[:exp] = exp.to_i
    JWT.encode(payload, SECRET_KEY)
  end


  def jwt_decode(token)
    decoded = JWT.decode(token, SECRET_KEY)[0]
    HashWithIndifferentAccess.new decoded
  end
  
end

{% endhighlight %}


<br>
10 . Create authenticate_request method in <span class="badge">app/controllers/application_controller.rb</span>.

{% highlight ruby %}

class ApplicationController < ActionController::API
  include JsonWebToken
  
  before_action :authenticate_request
  
  private
    def authenticate_request
      header = request.headers["Authorization"]
      header = header.split(" ").last if header
      decoded = jwt_decode(header)
      @current_user = User.find(decoded[:user_id])
    end

end

{% endhighlight %}


<br> 
11 . Create authentication controller and add the login method.

{% highlight terminal %}

$ rails g controller authentication

{% endhighlight %}


<br>
{% highlight ruby %}
# app/controllers/concerns/authentication_controller.rb

class AuthenticationController < ApplicationController
  skip_before_action :authenticate_request
  
  
  # POST /auth/login
  def login
    @user =User.find_by_email(params[:email])
    if @user&.authenticate(params[:password])
      token = jwt_encode(user_id: @user.id)
      render json: { token: token }, status: :ok
    else
      render json: { error: 'unauthorized' }, status: :unauthorized
    end 
  end

end

{% endhighlight %}


<br>
12 . Update routes

{% highlight ruby %}

Rails.application.routes.draw do  
  resources :users
  post "/auth/login", to: "authentication#login" 
end

{% endhighlight %}


<br>
### Test the application via postman

1 . Create a user

Notice the response containing the created user data at the bottom.

<img class="zoom-on-hover" srcset="
  {{site.baseurl}}/assets/img/posts/postman_1.png 1.3x,
  {{site.baseurl}}/assets/img/posts/postman_1.png 2.8x
" alt="missing image">


<br>
2 . Login

Notice the response containing the authorization token at the bottom.

<img class="zoom-on-hover" srcset="
  {{site.baseurl}}/assets/img/posts/postman_2.png 1.3x,
  {{site.baseurl}}/assets/img/posts/postman_2.png 2.8x
" alt="missing image">



<br>
*Thanks for reading, see you in the next one!*
