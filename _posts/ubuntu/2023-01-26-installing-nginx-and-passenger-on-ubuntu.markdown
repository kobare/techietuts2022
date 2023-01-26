---
layout: post
title:  "Installing Nginx and Passenger on Ubuntu"
author: Denis Kobare
date:   2023-01-26 12:30:00 +0300
img: /assets/img/svg/ubuntu.svg
categories: more-topics
sub_category: operating-systems
type: ubuntu
technology: nginx, passenger
permalink: "category/:categories/operating-systems/ubuntu/:year:month/:title"
---


### Introduction
This section describes the installation of Nginx and Passenger on Ubuntu. 



<br>
### 1. Install nginx

{% highlight terminal %}

$ sudo apt-get install nginx

{% endhighlight %}

<br>
### 2. Install Passenger packages

{% highlight terminal %}

# Install our PGP key and add HTTPS support for APT
$ sudo apt-get install -y dirmngr gnupg apt-transport-https ca-certificates curl

$ curl https://oss-binaries.phusionpassenger.com/auto-software-signing-gpg-key.txt | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/phusion.gpg >/dev/null

# Add our APT repository
$ sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger jammy main > /etc/apt/sources.list.d/passenger.list'

$ sudo apt-get update

# Install Passenger + Nginx module
$ sudo apt-get install -y libnginx-mod-http-passenger

{% endhighlight %}


<br>
### 3 . Enable the Passenger Nginx module

{% highlight terminal %}
$ if [ ! -f /etc/nginx/modules-enabled/50-mod-http-passenger.conf ]; then sudo ln -s /usr/share/nginx/modules-available/mod-http-passenger.load /etc/nginx/modules-enabled/50-mod-http-passenger.conf ; fi

$ sudo ls /etc/nginx/conf.d/mod-http-passenger.conf

{% endhighlight %}


<br>
### 4 . Point Passenger to the correct version of Ruby

{% highlight terminal %}

$ sudo nano /etc/nginx/conf.d/mod-http-passenger.conf

{% endhighlight %}


<br>
Change the <span class="badge">passenger_ruby</span> directive to match the following:

{% highlight text %}

passenger_ruby /home/username/.rbenv/shims/ruby;

# for example
passenger_ruby /home/ubuntu/.rbenv/shims/ruby;

{% endhighlight %}


<br>
### 5 . Start nginx

{% highlight terminal %}

$ sudo service nginx start

{% endhighlight %}

<br>
Confirm that NGINX is running by visiting your server's public IP address in your browser. You should be greeted with the "Welcome to NGINX" message.



<br>
### 6 . Add an nginx server for your application

{% highlight terminal %}

# Remove the default nginx server
$ sudo rm /etc/nginx/sites-enabled/default


# create a server for your application
$ sudo nano /etc/nginx/sites-enabled/myapp

{% endhighlight %}

<br>
The contents should resemble this:

{% highlight text %}

server {
listen 80;
listen [::]:80;
server_name _;
root /home/username/projects/myapp/current/public;
passenger_enabled on;
passenger_app_env production;
location /cable {
passenger_app_group_name myapp_websocket;
passenger_force_max_concurrent_requests_per_process 0;
}
# Allow uploads up to 100MB in size

  location ~ ^/(assets|packs) {
    expires max;
    gzip_static on;
  }
}


{% endhighlight %}

Replace <span class="badge">username</span> with the name of your user and 
change <span class="badge">myapp</span> to the name of your app. You will use 
this same folder later on when you define your Capistrano <span class="badge">deploy_to</span> folder.
Save the file.



<br>
### 7 . Reload NGINX to load the new server files.

{% highlight terminal %}

$ sudo service nginx reload

{% endhighlight %}


<br>
*That's it! Thanks for reading, see you in the next one!*
