---
layout: post
title:  "Nginx Commands Cheat Sheet"
author: Denis Kobare
date:   2023-01-26 15:45:00 +0300
img: /assets/img/svg/ubuntu.svg
categories: more-topics
sub_category: operating-systems
type: ubuntu
technology: nginx, passenger
permalink: "category/:categories/operating-systems/ubuntu/:year:month/:title"
---


### Introduction
The following are usefull commands for nginx on linux.



<br>
### Part A: Commands

1 . Check nginx version

{% highlight terminal %}

$ nginx -v

{% endhighlight %}


<br>
2 . Start nginx server

{% highlight terminal %}

$ sudo service nginx start

{% endhighlight %}


<br>
3 . Stop nginx server

{% highlight terminal %}

$ sudo service nginx stop

{% endhighlight %}


<br>
4 . Restart nginx server

{% highlight terminal %}

$ sudo service nginx restart

{% endhighlight %}



<br>
5 . Reload nginx server

{% highlight terminal %}

$ sudo service nginx reload

{% endhighlight %}


<br>
6 . Check nginx status

{% highlight terminal %}

$ sudo service nginx status

{% endhighlight %}



<br>
7 . Find syntax errors in the Nginx configuration file after making changes

{% highlight terminal %}

$ sudo nginx -t

# or

$ sudo nginx -t -c /path/to/the/file

{% endhighlight %}


<br>
### Part B: File locations

1 . Log Files

<span class="badge">/var/log/nginx/access.log</span> <br>
<span class="badge">/var/log/nginx/error.log</span>


<br>
2 . Global config file

<span class="badge">/etc/nginx/conf.d/mod-http-passenger.conf</span>


<br>
3 . Default server file
<span class="badge">/etc/nginx/sites-enabled/default</span>

<br>
*That's it! Thanks for reading, see you in the next one!*
