---
layout: post
title:  "Configure Git on Ubuntu 18.10 Cosmic Canimal"
date:   2020-06-06 14:17:06 +0300
img: ionic_side_menu.png
categories: configurations
technology: Github/Git
tutorial_type: How To
---

<div align="center" style="background-color:#B22222"> 
<img srcset="
  https://drive.google.com/uc?id=1JXnK4HNIJivvw_BuzpMZXKprxALKKNx3 3x,
  https://drive.google.com/uc?id=1JXnK4HNIJivvw_BuzpMZXKprxALKKNx3 6x
" alt="missing image">
</div>
<br>

Git is used as a version control system. In this tutorial we are going to set it up to match our Github account. If you don't already have a Github account, [register](https://github.com/){:target="_blank"} one.


`Ubuntu version: 18.10`

<h4 align="center" >STEP 1: <h5 align="center" >Account Configuration</h5></h4>

<div class="window">
  <div class="terminal">
    <h4># Replace my name and email address in the following steps with the ones you used for your Github account.</h4>
    <p class="command">git config --global color.ui true</p>

    <p class="command">git config --global user.name "YOUR NAME"</p>

    <p class="command">git config --global user.email "YOUR@EMAIL.com"</p>

    <h4># Generate SSH key</h4>
    <p class="command">ssh-keygen -t rsa -b 4096 -C "YOUR@EMAIL.com"</p>
  </div>
</div>
<br>

<h4 align="center" >STEP 2: <h5 align="center" >Copy the generated SSH key and add it to your Github account</h5></h4>

<div class="window">
  <div class="terminal">
    <h4># Copy SSH key. This command outputs your SSH key on the terminal. Highlight everything and copy.</h4>
    <p class="command">cat ~/.ssh/id_rsa.pub</p>
  </div>
</div>
<br>

Paste the output of the above command [here](https://github.com/settings/ssh){:target="_blank"} . 

<h4 align="center" >STEP 3: <h5 align="center" >Verify it Worked</h5></h4>

<div class="window">
  <div class="terminal">
    <p class="command">ssh -T git@github.com</p>

<h4>You should get a message like this:</h4>
<p class="log">
      <span>
        Hi user_name! You've successfully authenticated but GitHub does not provide shell access.
      </span>
    </p>
  </div>
</div>
<br>


That's it!

*See you in the next tuts*


