---
layout: post
title:  "Configure Git on Ubuntu 18.10 Cosmic Canimal"
author: Denis Kobare
date:   2020-06-06 14:17:06 +0300
img: /assets/img/svg/git.svg
categories: vcs
sub_category: github
type: tutorials
technology: Github/Git
permalink: "category/:categories/github/configurations/:year:month/:title"
---


Git is used as a version control system. In this tutorial we are going to set it up to match our Github account. If you don't already have a Github account, [register](https://github.com/){:target="_blank"} one.


`Ubuntu version: 18.10`

<h4 align="center" >STEP 1: <h5 align="center" >Account Configuration</h5></h4>

<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">

 <h5 class="hashed"># Replace name and email in the following steps with the ones you used for your Github account.</h5>
 <p class="console">git config --global color.ui true</p>

 <p class="console">git config --global user.name "YOUR NAME"</p>

 <p class="console">git config --global user.email "YOUR@EMAIL.com"</p>

 <h5 class="hashed"># Generate SSH key</h5>
 <p class="console">ssh-keygen -t rsa -b 4096 -C "YOUR@EMAIL.com"</p>

</div>
</section><br>


<h4 align="center" >STEP 2: <h5 align="center" >Copy the generated SSH key and add it to your Github account</h5></h4>

<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">

 <h5 class="hashed"># Copy SSH key. This command outputs your SSH key on the terminal. Highlight everything and copy.</h5>
 <p class="console">cat ~/.ssh/id_rsa.pub</p>

</div>
</section><br>


Paste the output of the above command [here](https://github.com/settings/ssh){:target="_blank"} . <br><br>

<h4 align="center" >STEP 3: <h5 align="center" >Verify it Worked</h5></h4>

<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">

 <p class="console">ssh -T git@github.com</p>
 <h5 class="hashed">Hi user_name! You've successfully authenticated but GitHub does not provide shell access.</h5>
 
</div>
</section><br>



That's it!

*See you in the next tuts*


