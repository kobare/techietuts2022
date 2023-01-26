---
layout: post
title:  "Ruby & Ruby on Rails Errors"
author: Denis Kobare
date:   2020-06-09 15:51:06 +0300
img: /assets/img/svg/ror.svg
categories: frameworks
sub_category: ruby-on-rails
type: tutorials
technology: Ruby/Rails
permalink: "category/:categories/ruby-on-rails/exceptions/:year:month/:title"
---


<h4 align="center" >ERROR TYPE: <h5 align="center" >Dependency Error</h5></h4>

<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">

 <h5 class="hashed"># Error when trying to run a jekyll project</h5>
 <p class="console">Dependency Error: Yikes! It looks like you don't have jekyll-commonmark-ghpages or one of its dependencies installed. In order to use Jekyll as currently configured, you'll need to install this gem. The full error message from Ruby is: 'incompatible library version - /home/koby/gems/gems/commonmarker-0.17.13/lib/commonmarker/commonmarker.so' If you run into trouble, you can find helpful resources at https://jekyllrb.com/help/!</p>

 <h5 class="hashed"># Step 1:</h5>
 <h5 class="hashed"># Warning! This will uninstall bundler and all other gems</h5>
 <h5 class="hashed"># a removes all versions</h5>
 <h5 class="hashed"># I ignores dependencies</h5>
 <h5 class="hashed"># x includes executables</h5>

 
 <h5 class="hashed">For Rubygems >= 2.1.0:</h5>
 <p class="console">gem uninstall -aIx</p>

 <h5 class="hashed">For Rubygems < 2.1.0:</h5>
 <p class="console">for i in `gem list --no-versions`; do gem uninstall -aIx $i; done</p>

 <h5 class="hashed"># If the above didn't work: You could also build out a new Gemfile and run bundle clean --force. This will remove all other gems that aren't included in the new Gemfile.</h5>
 <p class="console">bundle clean --force</p>

 <h5 class="hashed"># Step 2:</h5>
 
 <h5 class="hashed"># Now install the version of bundler required by your project</h5>
 <p class="console">gem install bundler:2.1.4</p>

 <h5 class="hashed"># Step 3:</h5>
 
 <h5 class="hashed">#After this everything should be ok!</h5>
 <p class="console">bundle install</p>
   
</div>
</section><br>



That's it!

*See you in the next tuts*


