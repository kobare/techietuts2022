---
layout: post
title:  "Add SSH Key to a Git Repository"
author: Denis Kobare
date:   2020-06-09 17:07:06 +0300
img: /assets/img/svg/github.svg
categories: vcs
sub_category: github
type: tutorials
technology: Github/Git
permalink: "category/:categories/github/configurations/:year:month/:title"
---


Git is used as a version control system. In this tutorial we are going to set up SSH to a repository. If you don't already have a Github account, [register](https://github.com/){:target="_blank"} one.



<h4 align="center" >STEP 1: <h5 align="center" >Generate the SSH Key</h5></h4>

<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">

 <h5 class="hashed"># Generate SSH key: Replace my email address in the following steps with the ones you used for your Github account.</h5>
 <p class="console">ssh-keygen -t rsa -b 4096 -C "YOUR@EMAIL.com"</p>
 
 <h5 class="hashed"># You will get a message similar to the one below</h5>

 <h5 class="hashed">#Enter the path and the file name after the prompt. Notice how the path matches the one above</h5>
 <p class="console">/home/pc_user/.ssh/name_of_your_key</p>

 <h5 class="hashed"># You will be prompted to enter a paraphrase. You can leave it blank and press enter if you like</h5>
 
 <h5 class="hashed"># Copy SSH key. This command outputs your SSH key on the terminal. Highlight everything and copy.</h5>
 <p class="console">cat ~/.ssh/name_of_your_key.pub</p>
   
</div>
</section><br>


<h4 align="center" >STEP 2: <h5 align="center" >Copy the generated SSH key and add it to your Github repository</h5></h4>
 You do that by opening your repository, go to the `settings` menu of that repository. Then click `deploy keys`, click `add` and paste the key.


That's it!

*See you in the next tuts*


