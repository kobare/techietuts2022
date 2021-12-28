---
layout: post
title:  "Install Ruby On Rails on Ubuntu 18.10 Cosmic Canimal"
date:   2020-06-06 14:17:06 +0300
img: ionic_side_menu.png
categories: installations
technology: Ruby/Rails
tutorial_type: How To
---

<div align="center" style="background-color:#B22222"> 
<img srcset="
  https://drive.google.com/uc?id=1BSD09VUFcwg9TCVsF846b8UnJtcJHfWa 3x,
  https://drive.google.com/uc?id=1BSD09VUFcwg9TCVsF846b8UnJtcJHfWa 6x
" alt="missing image">
</div>
<br>

In this tutorial we are going to set up Ruby and Ruby on Rails development environment on Ubuntu 18.10 Cosmic Canimal.

`Ubuntu version: 18.10`
`Ruby version: 2.7.1`
`Rails version: 6.0.2.2`



<h4 align="center" >STEP 1: <h5 align="center" >Install dependencies for Ruby and Rails</h5></h4>

<div class="window">
  <div class="terminal">
    <h4># Add the Node.js and Yarn repositories to our system before installing them to make sure we have everything necessary for Webpacker support in Rails.</h4>
    <p class="command">sudo apt install curl</p>
 
    <p class="command">curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -</p>

    <p class="command">curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -</p>

    <p class="command">echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list</p>

    <p class="command">sudo apt-get update</p>

    <p class="command">sudo apt-get install git-core zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev software-properties-common libffi-dev nodejs yarn</p>
  </div>
</div>
<br>

<h4 align="center" >STEP 2: <h5 align="center" >Install Ruby: Using rbenv</h5></h4>

<div class="window">
  <div class="terminal">
    <h4>#1. Install rbenv.</h4>
    <p class="command">cd</p>
 
    <p class="command">git clone https://github.com/rbenv/rbenv.git ~/.rbenv</p>

    <p class="command">echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc</p>

    <p class="command">echo 'eval "$(rbenv init -)"' >> ~/.bashrc</p>

    <p class="command">exec $SHELL</p>

    <h4>#2. Install ruby-build.</h4>
    <p class="command">git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build</p>

    <p class="command">echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc</p>

    <p class="command">exec $SHELL</p>

    <p class="command">rbenv install 2.7.1</p>

    <p class="command">rbenv rehash</p>

    <p class="command">rbenv global 2.7.1</p>

    <h4># Verify Ruby version</h4>
    <p class="command">ruby -v</p>
  </div>
</div>
<br>

<h4 align="center" >STEP 3: <h5 align="center" >Install bundler</h5></h4>

<div class="window">
  <div class="terminal">
    <p class="command">gem install bundler</p>
 
    <p class="command">rbenv rehash</p>
  </div>
</div>
<br>

<h4 align="center" >STEP 4: <h5 align="center" >Install Ruby on Rails</h5></h4>

<div class="window">
  <div class="terminal">
    <p class="command">gem install rails -v 6.0.2.2</p>
 
    <p class="command">rbenv rehash</p>

    <h4># Verify Rails version</h4>
    <p class="command">rails -v</p>
  </div>
</div>
<br>



That's it!

*See you in the next tuts*


