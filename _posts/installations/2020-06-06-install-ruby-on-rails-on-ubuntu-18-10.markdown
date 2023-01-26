---
layout: post
title:  "Install Ruby On Rails on Ubuntu 18.10 Cosmic Canimal"
author: Denis Kobare
date:   2020-06-06 14:17:06 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby/Rails
tutorial_type: How To
permalink: "category/:categories/ruby-on-rails/installations/:year:month/:title"
---


In this tutorial we are going to set up Ruby and Ruby on Rails development environment on Ubuntu 18.10 Cosmic Canimal.

`Ubuntu version: 18.10`
`Ruby version: 2.7.1`
`Rails version: 6.0.2.2`



<h4 align="center" >STEP 1: <h5 align="center" >Install dependencies for Ruby and Rails</h5></h4>

<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">

 <h5 class="hashed"># Add the Node.js and Yarn repositories to our system before installing them to make sure we have everything necessary for Webpacker support in Rails.</h5>
 <p class="console">sudo apt install curl</p>

 <p class="console">curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -</p>

 <p class="console">curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -</p>

 <p class="console">echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list</p>
 
 <p class="console">sudo apt-get update</p>

 <p class="console">sudo apt-get install git-core zlib1g-dev build-essential libssl-dev libreadline-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt1-dev libcurl4-openssl-dev software-properties-common libffi-dev nodejs yarn</p> 
</div>
</section><br>


<h4 align="center" >STEP 2: <h5 align="center" >Install Ruby: Using rbenv</h5></h4>

<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">

 <h5 class="hashed">#1. Install rbenv.</h5>
 <p class="console">cd</p>

 <p class="console">git clone https://github.com/rbenv/rbenv.git ~/.rbenv</p>

 <p class="console">echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc</p>

 <p class="console">echo 'eval "$(rbenv init -)"' >> ~/.bashrc</p>
 
 <p class="console">exec $SHELL</p>

 <h5 class="hashed">#2. Install ruby-build.</h5>
 <p class="console">git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build</p>

 <p class="console">echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc</p> 
 
 <p class="console">exec $SHELL</p>
 
 <p class="console">rbenv install 2.7.1</p>    
 
 <p class="console">rbenv rehash</p>    
 
 <p class="console">rbenv global 2.7.1</p>    

 <h5 class="hashed"># Verify Ruby version</h5> 
 <p class="console">ruby -v</p>            
</div>
</section><br>



<h4 align="center" >STEP 3: <h5 align="center" >Install bundler</h5></h4>

<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">

 <p class="console">gem install bundler</p>

 <p class="console">rbenv rehash</p>

</div>
</section><br>


<h4 align="center" >STEP 4: <h5 align="center" >Install Ruby on Rails</h5></h4>

<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">

 <p class="console">gem install rails -v 6.0.2.2</p>

 <p class="console">rbenv rehash</p>

 <h5 class="hashed"># Verify Rails version</h5> 
 <p class="console">rails -v</p> 

</div>
</section><br>


That's it!

*See you in the next tuts*


