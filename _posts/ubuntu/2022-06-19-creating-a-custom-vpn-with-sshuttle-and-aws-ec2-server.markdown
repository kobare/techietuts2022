---
layout: post
title:  "Creating a Custom VPN With sshuttle and AWS EC2 Server"
author: Denis Kobare
date:   2022-06-19 07:00:00 +0300
img: /assets/img/svg/ubuntu.svg
categories: more-topics
sub_category: operating-systems
type: ubuntu
technology: crontab
permalink: "category/:categories/operating-systems/ubuntu/:year:month/:title"
---


### Introduction
A virtual private network establishes a secure and encrypted connection between your computer and the internet, providing a private tunnel for your data in public networks.

While your ISP can't read your VPN traffic, your VPN can still read everything that leaves your device that isn't already encrypted. You then need to trust your VPN provider at least as well as you do your ISP. And your web browser even more unless you're using ToR browser. Even though your ISP will know you’re using a VPN the good news is that you’ll be shielded from those who want to know your information.

You can build your own VPN by installing and configuring a software on a local machine that tunnels into a cloud server.
sshuttle allows you to create a VPN connection from your machine to any remote server that you can connect to via ssh, as long as that server has python 2.3 or higher.
You must have root access on the local machine, but you dont need a root account on the server.

You can run sshuttle more than once simultaneously on a single client machine, connecting to a different server every time, so you can be on more than one VPN at once.
If run on a router, sshuttle can forward traffic for your entire subnet to the VPN. 


<br>
### Prerequisites

1. AWS EC2 instance running an Ubuntu server.
2. Python >=2.3 on the server.
3. ssh installed on the server and the local machine, and you must be able to ssh to the server.
4. Ubuntu desktop on a local machine, with root access.


<br>
### Install and Start Connection

<br>
1 .  Install sshuttle on your local machine

    $ sudo apt-get install sshuttle


<br>
2 .  Start connection to remote server

    $ sudo sshuttle [options...] [-r [username@]sshserver[:port]] <subnets...> 
    
    $ sudo sshuttle --dns -vvr techie@41.90.187.230 0/0


0/0 - all the subnets on the network

-vv - very verbose

-r - remote


<br>
3 .  Check your IP address

If you visit [ipchicken.com](https://ipchicken.com/){:target="_blank"}, you will see that the remote server IP is now your public IP. And just like that, you have your VPN!

Visit the [sshuttle](https://linux.die.net/man/8/sshuttle) manual page for more options.

*NB: Enable your dynamic ip if you’re trying to prevent being ddosd. By default AWS EC2 changes public IP whenever you restart the server and that's great!*

<br>
*Thanks for reading, see you in the next one!*
