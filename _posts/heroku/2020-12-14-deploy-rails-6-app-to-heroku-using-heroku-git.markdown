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
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
<p class="console">sudo snap install --classic heroku</p>

<h5 class="hashed"># Verify your installation</h5>
<p class="console">heroku --version</p>
  <h5 class="hashed">heroku/7.47.4 linux-x64 node-v12.16.2</h5>
</div>
</section><br><br><br>


### 2. Login to Heroku
After you install the CLI, run the heroku login command. You’ll be prompted to enter any key to go to your web browser to complete the login process. The CLI will then log you in automatically.
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
<p class="console">heroku login</p>
  <h5 class="hashed">
   heroku: Press any key to open up the browser to login or q to exit
   <br> ›   Warning: If browser does not open, visit
   <br> ›   https://cli-auth.heroku.com/auth/browser/***
   <br>heroku: Waiting for login...
   <br>Logging in... done
   <br>Logged in as me@example.com
  </h5>
</div>
</section>

<br><br>
If you’d prefer to stay in the CLI to enter your credentials, you may run heroku login -i
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
<p class="console">heroku login -i</p>
  <h5 class="hashed">
   heroku: Enter your login credentials
   <br> Email: me@example.com
   <br> Password: ***************
   <br> Two-factor code: ********
   <br> Logged in as me@heroku.com
  </h5>
</div>
</section>

<br><br>
### 3. Create Your App in Heroku
Open Heroku site, go to dashboard and [create](https://dashboard.heroku.com/new-app){:target="_blank"} a new app.


<br><br>
### 4. Update Git Remotes
Open your rails 6 app on terminal in your local machine and follow the commands below:

Update your git remotes to match the name of the app you created previously on Heroku site. Replace app-name with that name.
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
<h5 class="hashed"># go to the root of your project</h5>
<p class="console">cd ~/myapp</p>
<br>
<h5 class="hashed"># initialize git for your project</h5>
<p class="console">git init</p>
<h5 class="hashed">Initialized empty Git repository in .git/</h5>
<br>
<h5 class="hashed"># set up git remotes</h5>
<p class="console">heroku git:remote -a  app-name</p>
  <h5 class="hashed">
   module: @oclif/config@1.17.0
   <br>task: runHook prerun
   <br>plugin: heroku
   <br>root: /snap/heroku/4010
   <br>See more details with DEBUG=*
   <br>set git remote heroku to https://git.heroku.com/app-name.git
  </h5>
</div>
</section>


<br><br>
### 5. Deploy Your Rails 6 App
Stage, commit and push your rails 6 project to Heroku.
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
<p class="console">cd ~/myapp</p>
<h5 class="hashed"># verify git remote</h5>
<p class="console">git remote -v</p>
 <h5 class="hashed">
      heroku	https://git.heroku.com/app-name.git (fetch)
  <br>heroku	https://git.heroku.com/app-name.git (push)
 </h5>
<br>
<h5 class="hashed"># stage your project</h5>
<p class="console">git add .</p>
<h5 class="hashed"># commit</h5>
<p class="console">git commit -m 'Initial commit'</p>
<h5 class="hashed"># push</h5>
<p class="console">git push heroku master</p>
</div>
</section>


<br><br>
### 6. Setup your database
Set up database
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
<h5 class="hashed"># migrate database</h5>
<p class="console">heroku run rake db:migrate</p>
<h5 class="hashed"># open your deployed site</h5>
<p class="console">heroku open</p>
</div>
</section>
<br>

**Note:** In case of webpack error after deploying:

Create app/javascript/stylesheets folder and add in that folder, create an empty application.scss file

<br>Then open app/javascript/packs/application.js file and import added file to your pack js file with;

<br>import '../stylesheets/application';

<br>Let webpacker compile again all files with css/scss file. Then stage, commit and push again.
<br><br>

That's it! You have successfully deployed a Rails 6 app to Heroku.

*See you in the next tuts*


