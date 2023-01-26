---
layout: post
title:  "Git Cheatsheet"
author: Denis Kobare
date:   2020-06-09 17:07:06 +0300
img: /assets/img/svg/git.svg
categories: vcs
sub_category: github
type: tutorials
technology: Github/Git
permalink: "category/:categories/github/cheatsheet/:year:month/:title"

---


Git is used as a version control system. If you don't already have a Github account, [register](https://github.com/){:target="_blank"} one.

<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">

 <h5 class="hashed"># List SSH keys on your machine</h5>
 <p class="console">ls -al ~/.ssh</p>

 <h5 class="hashed"># Switching remote URL from SSH to HTTPS</h5>
 <p class="console">git remote set-url origin https://github.com/USERNAME/REPOSITORY.git</p>
 
 <h5 class="hashed"># Switching remote URL from HTTPS to SSH</h5>
 <p class="console">git remote set-url origin git@github.com:USERNAME/REPOSITORY.git</p>

 <h5 class="hashed"># Create a new branch</h5>
 <p class="console">git checkout -b your_branch_name</p>

 <h5 class="hashed"># List available branches</h5>
 <p class="console">git branch</p>
 
 <h5 class="hashed"># Navigate to a branch</h5>
 <p class="console">git checkout your_branch_name</p>

 <h5 class="hashed"># Check Git status</h5>
 <p class="console">git status</p>

 <h5 class="hashed"># Force unrelated merge</h5>
 <p class="console">git merge your_branch_name --allow-unrelated-histories</p>
  
</div>
</section><br>


That's it!

*See you in the next tuts*


