---
layout: post
title:  "Add SSH Key to a Git Repository"
date:   2020-06-09 17:07:06 +0300
img: ionic_side_menu.png
categories: cheat
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

Git is used as a version control system. In this tutorial we are going to set up SSH to a repository. If you don't already have a Github account, [register](https://github.com/){:target="_blank"} one.



<h4 align="center" >STEP 1: <h5 align="center" >Generate the SSH Key</h5></h4>

<div class="window">
  <div class="terminal">
    <h4># Generate SSH key: Replace my email address in the following steps with the ones you used for your Github account.</h4>
    <p class="command">ssh-keygen -t rsa -b 4096 -C "YOUR@EMAIL.com"</p>

    <h4># You will get a message similar to the one below</h4>
    <p class="log"><span>Enter file in which to save the key (/home/pc_user/.ssh/id_rsa): </span></p>

    <h4>#Enter the path and the file name after the prompt. Notice how the path matches the one above</h4>
    <p class="command">/home/pc_user/.ssh/name_of_your_key</p>

    <h4># You will be prompted to enter a paraphrase. You can leave it blank and press enter if you like</h4>

    <h4># Copy SSH key. This command outputs your SSH key on the terminal. Highlight everything and copy.</h4>
    <p class="command">cat ~/.ssh/name_of_your_key.pub</p>


  </div>
</div>
<br>

<h4 align="center" >STEP 2: <h5 align="center" >Copy the generated SSH key and add it to your Github repository</h5></h4>
 You do that by opening your repository, go to the `settings` menu of that repository. Then click `deploy keys`, click `add` and paste the key.


That's it!

*See you in the next tuts*


