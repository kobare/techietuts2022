---
layout: post
title:  "How to Deploy A Rails 6 App to Heroku: Using Heroku Git"
author: Denis Kobare
date:   2020-12-14 10:45:06 +0300
img: /assets/img/svg/heroku.svg
categories: cloud
sub_category: heroku
type: tutorials
technology: heroku/RoR
tutorial_type: How To
permalink: "category/:categories/heroku/tutorials/:year:month/:title"
tags:
- heroku
- deploy
- rails
- heroku-git
---


Heroku is a container-based cloud Platform as a Service (PaaS). Developers use Heroku to deploy, manage, and scale modern apps. The platform is elegant, flexible, and easy to use, offering developers the simplest path to getting their apps to market.


This tutorial will teach you how to deploy your rails 6 app to heroku.

  <table class="table table-bordered">
    <thead>
      <tr>
        <th colspan="5" style="text-align: center;">The guide assumes that you:</th>
      </tr>
    </thead>
    <tbody>
      <tr>
       <td>1. are using linux ubuntu as your OS</td>
      </tr>
      <tr>
       <td>2. have installed git on your machine</td>
      </tr>
      <tr>
       <td>3. have a Rails 6 project ready to be deployed</td>
      </tr>
      <tr>
       <td>4. are using postgres as your app’s database</td>
      </tr>
      <tr>
       <td>5. have opened an account with heroku </td>
      </tr>
      
    </tbody>
  </table><br>


 * If you don't have a Heroku account, [open an account](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwiAttb4ic3tAhWUsnEKHX63D6IQFjAAegQIARAD&url=https%3A%2F%2Fsignup.heroku.com%2F&usg=AOvVaw2kG63H0ONIjUijpD8T5fVY){:target="_blank"} with heroku 



Lets begin: 

### 1. Install Heroku CLI

{% highlight terminal %}

$ sudo snap install --classic heroku

 # Verify your installation
$ heroku --version
  heroku/7.47.4 linux-x64 node-v12.16.2

{% endhighlight %}<br>


### 2. Login to Heroku
After you install the CLI, run the heroku login command. You’ll be prompted to enter any key to go to your web browser to complete the login process. The CLI will then log you in automatically.

{% highlight terminal %}

$ heroku login

  heroku: Press any key to open up the browser to login or q to exit
        ›   Warning: If browser does not open, visit
        ›   https://cli-auth.heroku.com/auth/browser/***
          heroku: Waiting for login...
          Logging in... done
          Logged in as me@example.com

{% endhighlight %}<br>



If you’d prefer to stay in the CLI to enter your credentials, you may run heroku login -i

{% highlight terminal %}

$ heroku login -i

  heroku: Enter your login credentials
  Email: me@example.com
  Password: ***************
  Two-factor code: ********
  Logged in as me@heroku.com

{% endhighlight %}<br>



### 3. Create Your App in Heroku
Open Heroku site, go to dashboard and [create](https://dashboard.heroku.com/new-app){:target="_blank"} a new app.


<br><br>
### 4. Update Git Remotes
Open your rails 6 app on terminal in your local machine and follow the commands below:

Update your git remotes to match the name of the app you created previously on Heroku site. Replace app-name with that name.

{% highlight terminal %}

 # go to the root of your project
$ cd ~/myapp

 # initialize git for your project
$ git init
  
  Initialized empty Git repository in .git/
 
 # set up git remotes
$ heroku git:remote -a  app-name

  module: @oclif/config@1.17.0
  task: runHook prerun
  plugin: heroku
  root: /snap/heroku/4010
  See more details with DEBUG=*
  set git remote heroku to https://git.heroku.com/app-name.git

{% endhighlight %}<br>


<br><br>
### 5. Deploy Your Rails 6 App
Stage, commit and push your rails 6 project to Heroku.

{% highlight terminal %}

$ cd ~/myapp

 # verify git remote
$ git remote -v

  heroku    https://git.heroku.com/app-name.git (fetch)
  heroku    https://git.heroku.com/app-name.git (push)

 # stage your project
$ git add .

 # commit
$ git commit -m 'Initial commit'

 # push
$ git push heroku master

{% endhighlight %}<br>


<br><br>
### 6. Setup your database
Set up database

{% highlight terminal %}

 # migrate database
$ heroku run rake db:migrate

 # open your deployed site
$ heroku open

{% endhighlight %}<br>


**Note:** In case of webpack error after deploying:

Create app/javascript/stylesheets folder and add in that folder, create an empty application.scss file

<br>Then open app/javascript/packs/application.js file and import added file to your pack js file with;

<br>import '../stylesheets/application';

<br>Let webpacker compile again all files with css/scss file. Then stage, commit and push again.
<br><br>

That's it! You have successfully deployed a Rails 6 app to Heroku.

*See you in the next tuts*


