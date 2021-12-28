---
layout: post
title:  "rbenv Cheat Sheet"
author: Denis Kobare
date:   2020-06-06 14:17:06 +0300
img: /assets/img/svg/ruby_2.svg
categories: programming
sub_category: ruby
type: tutorials
technology: Ruby
permalink: "category/:categories/ruby/rbenv/cheatsheet/:year:month/:title"

---


<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
 <h5 class="hashed"># list all available versions</h5>
 <p class="console">rbenv install -l</p>

 <h5 class="hashed"># install a specific Ruby version</h5>
 <p class="console">rbenv install 2.7.1</p>

 <h5 class="hashed"># Install rbenv</h5>
 <p class="console">brew install rbenv</p>

 <h5 class="hashed"># Set a local application-specific Ruby version</h5>
 <p class="console">rbenv local 2.0.0</p>

 <h5 class="hashed"># Set a global Ruby version (default version)</h5>
 <p class="console">rbenv global 2.7.1</p>

 <h5 class="hashed"># Completely uninstall rbenv</h5>
 <p class="console">brew uninstall rbenv</p>

 <h5 class="hashed"># Set a shell-specific Ruby version by setting the RBENV_VERSION environment variable in your shell.</h5>
 <p class="console">rbenv shell 2.2.1</p>

 <h5 class="hashed"># List all Ruby versions known to rbenv</h5>
 <p class="console">rbenv versions</p>

 <h5 class="hashed"># Display the current active Ruby version (current opened project)</h5>
 <p class="console">rbenv version</p>

 <h5 class="hashed"># Run this command after you install a new version of Ruby or install a gem that provides commands if you're using rbenv</h5>
 <p class="console">rbenv rehash</p>

 <h5 class="hashed"># Display the full path to the executable that rbenv will invoke</h5>
 <p class="console">rbenv which irb</p>
  
</div>
</section><br>


That's it!

*See you in the next tuts*


