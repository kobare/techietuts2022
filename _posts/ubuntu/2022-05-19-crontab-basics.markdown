---
layout: post
title:  "Crontab Basics"
author: Denis Kobare
date:   2022-05-19 07:00:00 +0300
img: /assets/img/svg/ubuntu.svg
categories: os
sub_category: ubuntu
type: tutorials
technology: crontab
permalink: "category/:categories/ubuntu/tutorials/:year:month/:title"
---

### Introduction
In Linux, each user has their own crontab that can be used to schedule jobs that will be run as that user. Though crontabs are files in /var/spool/cron/crontabs, they are not intended to be edited directly. You're therefore required to view, update and delete a user crontab file using the crontab command. 


<br>
1 .  View current user's crontab.

    $ crontab -l


<br>
2 .  View root user's crontab.

    $ sudo crontab -l


<br>
3 .  View another user's crontab.

    $ crontab -u user_name_here -l


<br>
4 . Edit a user's crontab.
*NB: Always use crontab -e and never modify crontab files directly*

    $ crontab -u user_name_here -e


<br>
5 . Edit the root user's crontab.

*NB: Jobs scheduled in the root user crontab will be executed as root with all of its privileges.*
 
    $ sudo crontab -e


<br>
6 . Delete a user's crontab.

*NB: If you dont use the -i directive, you will not be prompted to confirm deletion.*

    $ crontab -u user_name_here -r -i


<br>
7 . To set a bash file to run in crontab, normally you'd make the file executable by doing chmod u+x your_bash_file.sh (executable for current user) or chmod a+x your_bash_file.sh (executable for all users). And then you'd open the crontab for editing and add this:

    # m h  dom mon dow   command
    00 13 * * * your_bash_file.sh

So that bash file will be executed at 13:00 PM everyday.

Suppose you haven't made the bash file executable, crontab could still execute it as long as you provide the path to the binary like so:

    # m h  dom mon dow   command
    00 13 * * * /bin/sh your_bash_file.sh    



<br>
8 . Run crontab command that reboots your machine at 22:00 on every day of the month on every month only from Monday through Friday.

    # m h  dom mon dow   command
    0 22 * * 1-5 reboot
    

<br>
9 . Suppose you want to run crontab command that executes a bash script at minute 23 past every 2nd hour between 0 (12am) and 20 (8pm), on every day of the month on every month and on every week day.
    
    # m h  dom mon dow   command
    23 0-20/2 * * * /bin/sh your_bash_file.sh  
    

<br>
10 . Set a time zone by adding a CRON_TZ declaration in your crontab.

    # CRON_TZ=America/Los_Angeles
    CRON_TZ=your_time_zone
    
    
If that doesn't work add this instead:

    # TZ=America/Los_Angeles
    TZ=your_time_zone 

Or you can set the time zone on the server all together via the terminal. First view the list of time zones available: 

    timedatectl list-timezones

Then set your desired time zone:

    sudo timedatectl set-timezone America/Los_Angeles

<br>
*Thanks for reading, see you in the next one!*
