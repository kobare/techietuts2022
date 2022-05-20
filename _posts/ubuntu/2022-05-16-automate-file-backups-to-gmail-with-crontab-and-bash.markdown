---
layout: post
title:  "Automate File Backups to Gmail with Bash and Crontab"
author: Denis Kobare
date:   2022-05-16 06:20:00 +0300
img: /assets/img/svg/ubuntu.svg
categories: os
sub_category: ubuntu
type: tutorials
technology: bash, crontab, gmail
permalink: "category/:categories/ubuntu/tutorials/:year:month/:title"
---

### Introduction

Data backup is the practice of copying data from a primary to a secondary location that you use for recovery in case your original data is lost or corrupted.

This document will describe how to set up an automated mysql database backup system that not only stores the backup file on the local machine, but also sends the zipped file to an email address at a given time on a daily basis.

You will need a gmail account that'll be used to automatically send backup files to recipients.

This will work on linux distros, both server and desktop. The steps on here were tested on an ubuntu 18.04 server and desktop.  

<br>
### a) Install mailutils

1 . Install mailutils from apt:

    sudo apt install mailutils


<br>
2 . In the "General type of mail configuration:" prompt, choose "Internet Site" and press enter.

<img class="zoom-on-hover" srcset="
  {{site.baseurl}}/assets/img/posts/postfix_configuration_1.png 1.1x,
  {{site.baseurl}}/assets/img/posts/postfix_configuration_1.png 2x
" alt="missing image">


<br>
3 . A postfix configuration prompt will open. On the "System mail name:" prompt, enter gmail.com

<img class="zoom-on-hover" srcset="
  {{site.baseurl}}/assets/img/posts/postfix_configuration_2.png.png 2.3x,
  {{site.baseurl}}/assets/img/posts/postfix_configuration_2.png.png 3x
" alt="missing image">


<br>
### b) Setup Gmail App Passwords
ssmtp will use your gmail account to send mail, but you can not use your normal gmail password with external applications. Gmail has a nifty feature called [App passwords](https://myaccount.google.com/apppasswords){:target="_blank"} that lets you sign in to your Google Account from apps on devices that don't support 2-Step Verification. Click [here](https://myaccount.google.com/apppasswords){:target="_blank"} to set up App passwords. On the page, select 'Mail' under the 'select app' tab, then enter a custom name under the 'select device' tab.

Click on the 'Generate' button to generate the password. Just like your normal password, this app password grants complete access to your Google Account. Copy 16-character password shown, you will need it for the next step.



<br>
### c) Install ssmtp

1 . Install ssmtp:

    sudo apt-get install ssmtp


<br>
2 . Edit ssmtp.conf file:

    sudo nano /etc/ssmtp/ssmtp.conf

Replace the placeholders (google_apps_password_here and your_account) with your details. 
Your file should resemble this (your hostname will be different):
    
{% highlight conf %}

#
# Config file for sSMTP sendmail
#
# The person who gets all mail for userids < 1000
# Make this empty to disable rewriting.
root=your_account@gmail.com:smtp.gmail.com:587

# The place where the mail goes. The actual machine name is required no 
# MX records are consulted. Commonly mailhosts are named mail.domain.com
mailhub=smtp.gmail.com:587

# Where will the mail seem to come from?
rewriteDomain=gmail.com

# The full hostname
hostname=Techietuts-HP-Compaq-PC

# Are users allowed to set their own From: address?
# YES - Allow the user to specify their own From: address
# NO - Use the system generated From: address
FromLineOverride=YES

Authuser=your_account@gmail.com
Authpass=google_apps_password_here
UseTLS=YES
UseSTARTTLS=YES

{% endhighlight %}    
    

<br>
3 . Edit revaliases file:

    sudo nano /etc/ssmtp/revaliases

Add this (remember to replace the place holder):

{% highlight text %}

# sSMTP aliases
# 
# Format:	local_account:outgoing_address:mailhub
#
# Example: root:your_login@your.domain:mailhub.your.domain[:port]
# where [:port] is an optional port number that defaults to 25.
root:your_account@gmail.com:smtp.gmail.com:587

{% endhighlight %}


<br>
### d) Create the bash script responsible for creating mysql dumps

Typically, you'd put it in:

    /usr/local/sbin/
    
1 . cd into /usr/local/sbin/ and create a bash file. Name it backup_mysql.sh

    sudo nano backup_mysql.sh  
 
2 . Add this backup script to backup_mysql.sh then save and close it:

{% highlight bash %}

#! /bin/bash

# Backup storage directory 
#backupfolder=/var/backups
mkdir -p /var/backups/databases
local_backupfolder=/var/backups/databases


# The email address where the zipped file will be sent to 
recipient_email=some_account@gmail.com

# MySQL user
user=root

# MySQL password
password=your_mysql_password

# Number of days to store the backup 
keep_day=2 

sqlfile=$local_backupfolder/your_file_name_$(date +%d_%m_%Y_%H_%M_%S).sql
zipfile=$local_backupfolder/your_file_name_$(date +%d_%m_%Y_%H_%M_%S).zip 

# Create a backup 
# sudo mysqldump -u $user -p$password --all-databases > $sqlfile 
mysqldump -u $user -p$password your_database_name > $sqlfile 

# Compress backup 
zip $zipfile $sqlfile 

# Delete the uncompresed file
rm $sqlfile 

# Mail compressed backup file
mailx -s "DB Backup" $recipient_email -A $zipfile 

# Delete old backups 
find $local_backupfolder -mtime +$keep_day -delete


{% endhighlight %}


<br>
### e) Create the cron job

1 . Open crontab as root 

    sudo crontab -e


<br>
2 . Add this at the end of the file:

This cron job will run the backup script created in the previous step, everyday at 4:30 PM.

{% highlight crontab %}

30 16 * * * /bin/sh /usr/local/sbin/backup_mysql.sh 

{% endhighlight %}

With this, the zipped backup file will be sent to the recipient email you set in the bash file, every day at 4:30 PM. Feel free to change cron job time to your current time to test it. 

<br>
*Thanks for reading, see you in the next one!*
