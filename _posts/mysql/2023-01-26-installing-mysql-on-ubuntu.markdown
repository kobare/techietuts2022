---
layout: post
title:  "Installing MySQL on Ubuntu"
author: Denis Kobare
date:   2023-01-26 12:00:00 +0300
img: /assets/img/svg/mysql.svg
categories: databases
sub_category: mysql
type: tutorials
technology: MySQL
permalink: "category/:categories/mysql/tutorials/:year:month/:title"
---


<br>
### 1 . Install MySQL

Install both the server and client libraries so we can compile the mysql2 rubygem

{% highlight sql %}

$ sudo apt-get install mysql-server mysql-client libmysqlclient-dev

$ sudo mysql_secure_installation

{% endhighlight %}


<br>
### 2 . Remove sudo login

{% highlight sql %}

# login to mysql
$ sudo mysql -u root -p


$ show variables like 'validate_password%';


# you can set the password policy level lower, for example

$ set global validate_password.length = 6;

$ set global validate_password.number_count = 0;


# set the password

$ alter user 'root'@'localhost' identified with mysql_native_password by 'yourpassword';

# Or use caching_sha2_password instead (recommended)
$ ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'yourpassword';


$ flush privileges;

{% endhighlight %}



<br>
*Thanks for reading, see you in the next one!*
