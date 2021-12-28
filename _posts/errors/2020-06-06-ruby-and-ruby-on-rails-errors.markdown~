---
layout: post
title:  "Ruby & Ruby on Rails Errors"
date:   2020-06-09 15:51:06 +0300
img: ionic_side_menu.png
categories: errors
technology: Ruby/Rails
tutorial_type: How To
---

<div align="center" style="background-color:#000"> 
<img srcset="
  https://drive.google.com/uc?id=1oFSYHqhPRaK1E4NhgzDG42PtFeXP3-Sl 3x,
  https://drive.google.com/uc?id=1oFSYHqhPRaK1E4NhgzDG42PtFeXP3-Sl 6x
" alt="missing image">
</div>
<br>


<h4 align="center" >ERROR TYPE: <h5 align="center" >Dependency Error</h5></h4>

<div class="window">
  <div class="terminal">
    <h4># Error when trying to run a jekyll project</h4>
    <p class="command">Dependency Error: Yikes! It looks like you don't have jekyll-commonmark-ghpages or one of its dependencies installed. In order to use Jekyll as currently configured, you'll need to install this gem. The full error message from Ruby is: 'incompatible library version - /home/koby/gems/gems/commonmarker-0.17.13/lib/commonmarker/commonmarker.so' If you run into trouble, you can find helpful resources at https://jekyllrb.com/help/!</p>

    <h4># Step 1:</h4>
    <h4># Warning! This will uninstall bundler and all other gems</h4>
    <h3># a removes all versions</h3>
    <h3># I ignores dependencies</h3>
    <h3># x includes executables</h3>

    <h4>For Rubygems >= 2.1.0</h4>
    <p class="command">gem uninstall -aIx</p>

    <h4>For Rubygems < 2.1.0</h4>
    <p class="command">for i in `gem list --no-versions`; do gem uninstall -aIx $i; done</p>

    <h4># If the above didn't work: You could also build out a new Gemfile and run bundle clean --force. This will remove all other gems that aren't included in the new Gemfile.</h4>
    <p class="command">bundle clean --force</p>

    <h4># Step 2:</h4>
    <h4># Now install the version of bundler required by your project</h4>
    <p class="command">gem install bundler:2.1.4</p>

    <h4># Step 3:</h4>
    <h4>#After this everything should be ok!</h4>
    <p class="command">bundle install</p>
  </div>
</div>
<br>



That's it!

*See you in the next tuts*


