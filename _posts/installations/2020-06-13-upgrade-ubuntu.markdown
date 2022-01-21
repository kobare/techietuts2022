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


## METHOD 1:

### Update all installed packages

{% highlight terminal %}

$ sudo apt update && sudo apt upgrade

 # Remove unused kernels to free up the space from /boot partition
$ sudo apt autoremove

$ sudo reboot  

{% endhighlight %}<br>

  

### Installation Process

{% highlight terminal %}

 # 1. Execute below command to install “update-manager-core“, as it is required for upgrade, though on most of the systems it should be installed by default. In case it is not installed run below command

$ sudo apt install update-manager-core -y

 # 2. Begin upgrade process
$ do-release-upgrade
  
{% endhighlight %}<br>


## METHOD 2

We cannot upgrade ubuntu from a version that has reached it's end of life support with Method 1 above. But there's a workaround which involves changing a few files and upgrading incrementally from one version to the next i.e we go from `18.10` to `19.04` to `19.10` and finally to `20.04`

### Point to the correct repository
 	
{% highlight terminal %}

sudo nano /etc/apt/sources.list

{% endhighlight %}<br>


The above file originally has these lines of code among others:

NB: Keep in mind, our ubuntu here is 18.10 hence the code name "cosmic". And this varies with the ubuntu version you are using. 

{% highlight nano %}

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

{% endhighlight %}<br>


If the ubuntu version we are upgrading to is past it's end of life support it is "obsolete" and it's archive has been moved to "old-releases", so you will need to edit /etc/apt/sources.list to point to: "deb http://old-releases.ubuntu.com/ubuntu" like so:


{% highlight nano %}

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

{% endhighlight %}<br>					  						  		


### Installation Process

{% highlight nano %}
 	
 #1. Open release-upgrades file and make sure it has "Prompt=normal"
$ sudo nano /etc/update-manager/release-upgrades

 # 2. Edit the cached update list which is in: ~/.cache/update-manager-core/meta-release. Make sure the supported flag of your ubuntu code name reads "1". e.g in our case we change "cosmic" supported flag from "0" to "1" 
$ sudo nano ~/.cache/update-manager-core/meta-release

{% endhighlight %}<br>					  						  		

 
### Update all installed packages
 	
{% highlight nano %}

$ sudo apt update
$ sudo apt upgrade
$ sudo apt autoremove
 
 # Restart your machine 
$ sudo reboot                  

{% endhighlight %}<br>					  						  		


### Upgrade Process

{% highlight nano %}
 	
 # 1. Open release-upgrades file and make sure it has "Prompt=normal"
$ sudo nano /etc/update-manager/release-upgrades

 # 2. Edit the cached update list which is in: ~/.cache/update-manager-core/meta-release. Make sure the supported flag of your ubuntu code name reads "1". e.g in our case we change "disco" supported flag from "0" to "1" 
$ sudo nano ~/.cache/update-manager-core/meta-release

 # 3. Begin upgrade process
$ do-release-upgrade                 

{% endhighlight %}<br>					  						  		

 

We repeat steps 1 through 4 for the next versions. Arduous but works like a charm.

That's it!

*See you in the next tuts*



