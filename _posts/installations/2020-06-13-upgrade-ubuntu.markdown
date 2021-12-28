---
layout: post
title:  "Upgrade Ubuntu"
author: Denis Kobare
date:   2020-06-13 14:22:00 +0300
img: /assets/img/svg/ubuntu_2.svg
categories: os
sub_category: ubuntu
type: tutorials
technology: Linux/Ubuntu
tutorial_type: How To
permalink: "category/:categories/ubuntu/installations/:year:month/:title"
---




In this tutorial we are going to upgrade ubuntu from `ubuntu 18.10` to `ubuntu 20.04 LTS`.

NB: If the version of ubuntu we are upgrading from is past it's end of life support, we follow Method 2 otherwise we follow Method 1.

<section class="ftco-section ftco-no-pb ftco-no-pt" id="chapter-section">
<div class="container">
<div class="row justify-content-center py-5 mt-5">
<div class="col-md-12 heading-section text-center ftco-animate">
<h2 id="method-1" class="mb-4">METHOD 1</h2>
</div>
</div>
<div class="row">
<div class="col-md-3 mb-4">
<nav id="navi">
    <ul>
     <li><a href="#method-1">METHOD 1</a>
      <ul>
      <li><a href="#page-1">Step 1</a></li>
      <li><a href="#page-2">Step 2</a></li>
      </ul>
     </li>
     <li><a href="#method-2">METHOD 2</a>
      <ul>
      <li><a href="#page-3">Step 1</a></li>
      <li><a href="#page-4">Step 2</a></li>
      <li><a href="#page-5">Step 3</a></li>
      <li><a href="#page-6">Step 4</a></li>            
      </ul>
     </li>
    </ul>
  </nav>
</div>
<div class="col-md-9">
  <div id="page-1" class= "page bg-light one">
  	<h2 class="heading center">Update all installed packages</h2>
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
<p class="console">sudo apt update</p>
<p class="console">sudo apt upgrade</p>

<h5 class="hashed"># Remove unused kernels to free up the space from /boot partition</h5>
<p class="console">sudo apt autoremove</p>

<p class="console">sudo reboot</p>   
</div>
</section>					  						  	
  </div>
  
  <div id="page-2" class= "page bg-light two">
  	<h2 class="heading center">Installation Process</h2>
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
    <h5 class="hashed">#1. Execute below command to install “update-manager-core“, as it is required for upgrade, though on most of the systems it should be installed by default. In case it is not installed run below command</h5>

<p class="console">sudo apt install update-manager-core -y</p>

    <h5 class="hashed">#2. Begin upgrade process</h5>
<p class="console">do-release-upgrade</p>
  
</div>
</section>					  						  		
  </div>

<div class="row justify-content-center py-5 mt-5">
<div class="col-md-12 heading-section text-center ftco-animate">
<h2 id="method-2" class="mb-4">METHOD 2</h2>
</div>
</div>
<p>We cannot upgrade ubuntu from a version that has reached it's end of life support with Method 1 above. But there's a workaround which involves changing a few files and upgrading incrementally from one version to the next i.e we go from `18.10` to `19.04` to `19.10` and finally to `20.04`
</p> 
  <div id="page-3" class= "page bg-light two">
  	<h2 class="heading center">Point to the correct repository</h2>
 	
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
<p class="console">sudo nano /etc/apt/sources.list</p> 
</div>
</section>
The above file originally has these lines of code among others:

NB: Keep in mind, our ubuntu here is 18.10 hence the code name "cosmic". And this varies with the ubuntu version you are using. 

{% highlight ruby linenos %}

deb http://ke.archive.ubuntu.com/ubuntu/ cosmic main restricted
deb http://ke.archive.ubuntu.com/ubuntu/ cosmic-updates main restricted
deb http://ke.archive.ubuntu.com/ubuntu/ cosmic universe
deb http://ke.archive.ubuntu.com/ubuntu/ cosmic-updates universe
deb http://ke.archive.ubuntu.com/ubuntu/ cosmic multiverse
deb http://ke.archive.ubuntu.com/ubuntu/ cosmic-updates multiverse
deb http://ke.archive.ubuntu.com/ubuntu/ cosmic-backports main restricted universe multiverse
deb http://security.ubuntu.com/ubuntu cosmic-security main restricted
deb http://security.ubuntu.com/ubuntu cosmic-security universe
deb http://security.ubuntu.com/ubuntu cosmic-security multiverse

{% endhighlight %}


If the ubuntu version we are upgrading to is past it's end of life support it is "obsolete" and it's archive has been moved to "old-releases", so you will need to edit /etc/apt/sources.list to point to: "deb http://old-releases.ubuntu.com/ubuntu" like so:


{% highlight ruby linenos %}

# deb http://ke.archive.ubuntu.com/ubuntu/ cosmic main restricted #current version. comment it out
#the version we are upgrading to.Uncomment this if disco is still supported, otherwise use the last line
# deb http://ke.archive.ubuntu.com/ubuntu/ disco main restricted 
deb http://old-releases.ubuntu.com/ubuntu/ disco main restricted

# deb http://ke.archive.ubuntu.com/ubuntu/ cosmic-updates main restricted #current version. comment it out
#the version we are upgrading to. Uncomment this if disco is still supported, otherwise use the last line
# deb http://ke.archive.ubuntu.com/ubuntu/ disco-updates main restricted 
 deb http://old-releases.ubuntu.com/ubuntu/ disco-updates main restricted

# deb http://ke.archive.ubuntu.com/ubuntu/ cosmic universe #current version. comment it out
#the version we are upgrading to. Uncomment this if disco is still supported, otherwise use the last line
# deb http://ke.archive.ubuntu.com/ubuntu/ disco universe 
deb http://old-releases.ubuntu.com/ubuntu/ disco universe


# deb http://ke.archive.ubuntu.com/ubuntu/ cosmic-updates universe #current version. comment it out
#the version we are upgrading to. Uncomment this if disco is still supported, otherwise use the last line
# deb http://ke.archive.ubuntu.com/ubuntu/ disco-updates universe 
 deb http://old-releases.ubuntu.com/ubuntu/ disco-updates universe

# deb http://ke.archive.ubuntu.com/ubuntu/ cosmic multiverse #current version. comment it out
#the version we are upgrading to. Uncomment this if disco is still supported, otherwise use the last line
# deb http://ke.archive.ubuntu.com/ubuntu/ disco multiverse 
 deb http://old-releases.ubuntu.com/ubuntu/ disco multiverse

# deb http://ke.archive.ubuntu.com/ubuntu/ cosmic-updates multiverse #current version. comment it out
#the version we are upgrading to. Uncomment this if disco is still supported, otherwise use the last line
# deb http://ke.archive.ubuntu.com/ubuntu/ disco-updates multiverse 
 deb http://old-releases.ubuntu.com/ubuntu/ disco-updates multiverse


# deb http://ke.archive.ubuntu.com/ubuntu/ cosmic-backports main restricted universe multiverse #current version. comment it out
#the version we are upgrading to. Uncomment this if disco is still supported, otherwise use the last line
# deb http://ke.archive.ubuntu.com/ubuntu/ disco-backports main restricted universe multiverse 
 deb http://old-releases.ubuntu.com/ubuntu/ disco-backports main restricted universe multiverse

# deb http://security.ubuntu.com/ubuntu cosmic-security main restricted #current version. comment it out
#the version we are upgrading to. Uncomment this if disco is still supported, otherwise use the last line
# deb http://security.ubuntu.com/ubuntu disco-security main restricted 
 deb http://old-releases.ubuntu.com/ubuntu/ disco-security main restricted

# deb http://security.ubuntu.com/ubuntu cosmic-security universe #current version. comment it out
#the version we are upgrading to. Uncomment this if disco is still supported, otherwise use the last line
# deb http://security.ubuntu.com/ubuntu disco-security universe 
 deb http://old-releases.ubuntu.com/ubuntu/ cosmic-security universe

# deb http://security.ubuntu.com/ubuntu cosmic-security multiverse #current version. comment it out
#the version we are upgrading to. Uncomment this if disco is still supported, otherwise use the last line
# deb http://security.ubuntu.com/ubuntu disco-security multiverse 
 deb http://old-releases.ubuntu.com/ubuntu/ disco-security multiverse

{% endhighlight %}					  						  		
</div>

  <div id="page-4" class= "page bg-light two">
  	<h2 class="heading center">Installation Process</h2>
 	
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
    <h5 class="hashed">#1. Open release-upgrades file and make sure it has "Prompt=normal"</h5>
    <p class="console">sudo nano /etc/update-manager/release-upgrades</p>

    <h5 class="hashed">#2. Edit the cached update list which is in: ~/.cache/update-manager-core/meta-release. Make sure the supported flag of your ubuntu code name reads "1". e.g in our case we change "cosmic" supported flag from "0" to "1"</h5> 
    <p class="console">sudo nano ~/.cache/update-manager-core/meta-release</p>
</div>
</section>
</div>  
 
  <div id="page-5" class= "page bg-light two">
  	<h2 class="heading center">Update all installed packages</h2>
 	
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
    <p class="console">sudo apt update</p>
    <p class="console">sudo apt upgrade</p>
    <p class="console">sudo apt autoremove</p>
    <h5 class="hashed"># Restart your machine</h5>     
    <p class="console">sudo reboot</p>                    
</div>
</section>
</div> 

  <div id="page-6" class= "page bg-light two">
  	<h2 class="heading center">Upgrade Process</h2>
 	
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
    <h5 class="hashed">#1. Open release-upgrades file and make sure it has "Prompt=normal"</h5 >
    <p class="console">sudo nano /etc/update-manager/release-upgrades</p>

    <h5 class="hashed">#2. Edit the cached update list which is in: ~/.cache/update-manager-core/meta-release. Make sure the supported flag of your ubuntu code name reads "1". e.g in our case we change "disco" supported flag from "0" to "1"</h5 > 
    <p class="console">sudo nano ~/.cache/update-manager-core/meta-release</p>

    <h5 class="hashed">#3. Begin upgrade process</h5 >
    <p class="console">do-release-upgrade</p>                 
</div>
</section>
</div>  
 
</div>
</div>
</div>
</section>

We repeat steps 1 through 4 for the next versions. Arduous but works like a charm.

That's it!

*See you in the next tuts*



