---
layout: post
title:  "Upgrade Ubuntu"
date:   2020-06-13 14:22:00 +0300
categories: installations
technology: Linux/Ubuntu
tutorial_type: How To
---

<div align="center" style="background-color:#fff"> 
<img srcset="
  https://drive.google.com/uc?id=1OYNUMOx_DxtCedG50i_Qt7zkdThk4m_K 2x,
  https://drive.google.com/uc?id=1OYNUMOx_DxtCedG50i_Qt7zkdThk4m_K 4x
" alt="missing image">
</div>
<br>

In this tutorial we are going to upgrade ubuntu from `ubuntu 18.10` to `ubuntu 20.04 LTS`.

NB: If the version of ubuntu we are upgrading from is past it's end of life support, we follow Method 2 otherwise we follow Method 1.

### METHOD 1: 

<h4 align="center" >STEP 1: <h5 align="center" >Update all installed packages</h5></h4>

<div class="window">
  <div class="terminal">
    <p class="command">sudo apt update</p>
 
    <p class="command">sudo apt upgrade</p>

    <h4># Remove unused kernels to free up the space from /boot partition</h4>
    <p class="command">sudo apt autoremove</p>

    <p class="command">sudo reboot</p>
  </div>
</div>
<br>

<h4 align="center" >STEP 2: <h5 align="center" >Installation Process</h5></h4>

<div class="window">
  <div class="terminal">
    <h4>#1. Execute below command to install “update-manager-core“, as it is required for upgrade, though on most of the systems it should be installed by default. In case it is not installed run below command</h4>
    <p class="command">sudo apt install update-manager-core -y</p>

    <h4>#2. Begin upgrade process</h4>
    <p class="command">do-release-upgrade</p>
  </div>
</div>
<br>

 
### METHOD 2:

We cannot upgrade ubuntu from a version that has reached it's end of life support with Method 1 above. But there's a workaround which involves changing a few files and upgrading incrementally from one version to the next i.e we go from `18.10` to `19.04` to `19.10` and finally to `20.04`

<h4 align="center" >STEP 1: <h5 align="center" >Point to the correct repository</h5></h4>

<div class="window">
  <div class="terminal">
    <p class="command">sudo nano /etc/apt/sources.list</p>
  </div>
</div>
<br>

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


<h4 align="center" >STEP 2: <h5 align="center" >Installation Process</h5></h4>

<div class="window">
  <div class="terminal">
    <h4>#1. Open release-upgrades file and make sure it has "Prompt=normal"</h4>
    <p class="command">sudo nano /etc/update-manager/release-upgrades</p>

    <h4>#2. Edit the cached update list which is in: ~/.cache/update-manager-core/meta-release. Make sure the supported flag of your ubuntu code name reads "1". e.g in our case we change "cosmic" supported flag from "0" to "1"</h4> 
    <p class="command">sudo nano ~/.cache/update-manager-core/meta-release</p>
  </div>
</div>
<br>


<h4 align="center" >STEP 3: <h5 align="center" >Update all installed packages</h5></h4>

<div class="window">
  <div class="terminal">
    <h4># </h4>
    <p class="command">sudo apt update</p>
 
    <p class="command">sudo apt upgrade</p>

    <p class="command">sudo apt autoremove</p>

    <p class="command">sudo reboot</p>
  </div>
</div>
<br>


<h4 align="center" >STEP 4: <h5 align="center" >Installation Process</h5></h4>

<div class="window">
  <div class="terminal">
    <h4>#1. Open release-upgrades file and make sure it has "Prompt=normal"</h4>
    <p class="command">sudo nano /etc/update-manager/release-upgrades</p>

    <h4>#2. Edit the cached update list which is in: ~/.cache/update-manager-core/meta-release. Make sure the supported flag of your ubuntu code name reads "1". e.g in our case we change "disco" supported flag from "0" to "1"</h4> 
    <p class="command">sudo nano ~/.cache/update-manager-core/meta-release</p>

    <h4>#3. Begin upgrade process</h4>
    <p class="command">do-release-upgrade</p>
  </div>
</div>
<br>

We repeat steps 1 through 4 for the next versions. Arduous but works like a charm.

That's it!

*See you in the next tuts*


