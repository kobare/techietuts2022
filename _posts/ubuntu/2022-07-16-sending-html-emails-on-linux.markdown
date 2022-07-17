---
layout: post
title:  "Sending HTML Emails on Linux"
author: Denis Kobare
date:   2022-07-16 14:00:00 +0300
img: /assets/img/svg/ubuntu.svg
categories: more-topics
sub_category: operating-systems
type: ubuntu
technology: bash, crontab, gmail
permalink: "category/:categories/operating-systems/ubuntu/:year:month/:title"
---


### Introduction

HTML emails are a engaging and generally work well for marketing emails. 
They allow you to design emails that match your brand and give your readers a more visually engaging experience.

Majority of mail clients do not support HTML in emails and as such, it can be a pain designing HTML emails that render
properly on the recipients view.

While there are multiple services that can handle this task for you, you may want to do it your self if you value control and flexibility.
This section will guide you on how to achieve that.
 

<br>
### Prerequisites
1. Linux distro e.g Ubuntu
2. Gmail (You may try other options)


<br>
### a) Install mailutils

1 . Install mailutils from apt:

    sudo apt install mailutils


<br>
2 . In the "General type of mail configuration:" prompt, choose "Internet Site" and press enter.

<img class="zoom-on-hover mobile-image" srcset="
  {{site.baseurl}}/assets/img/posts/postfix_configuration_1.png 1.1x,
  {{site.baseurl}}/assets/img/posts/postfix_configuration_1.png 2x
" alt="missing image">


<br>
3 . A postfix configuration prompt will open. On the "System mail name:" prompt, enter gmail.com

<img class="zoom-on-hover mobile-image" srcset="
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
### d) Create the HTML Template

Here's an example of HTML email template. Notice that inline css was used, and that's because mail clients will strip out all stylesheets except for inline styles.

1 . Copy this code and save it in a file called html_email_example.html:

{% highlight html %}

  <div class="email-container" style="margin-top: 2rem; margin-right: auto; margin-left: auto; overflow: hidden; max-width: 37.5rem; background: #fff; border-radius: 0.125rem; box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23); transition: box-shadow 0.25s cubic-bezier(0.4, 0, 1, 1); border: 1px #999 solid;">
    <div class="email__header">
      <img class="header__thumbnail" src="https://www.techietuts.com/assets/img/footer-image-4.jpg" alt="missing image" style="max-width: 100%; -o-object-fit: fill; object-fit: fill;">
     <div class="centered" style="position: absolute; top: 15%; left: 50%; transform: translate(-50%, -50%); color: #fff; font-weight: 800;">Ruby, JS, CSS, Python and More...</div>
    </div>
  
    <div class="email__body" style="padding-top: 1rem; padding-right: 3rem; padding-bottom: 1rem; padding-left: 3rem;">
      <h1 class="body__heading" style="margin-bottom: 0.5rem; font-size: 1.3125rem; font-weight: 700; letter-spacing: 1px; text-transform: uppercase; color: #141414;">Refactoring Ruby Code: Example 1</h1>
      
      <small class="body__meta" style="display: inline-block; margin-bottom: 0.728rem; font-size: 0.8125rem; font-weight: 600; letter-spacing: 1px; color: rgba(20, 20, 20, 0.4);">Published on March 2022, Reading time: 6 minutes</small>
      <h3>Definition</h3>
      <p class="body__text" style="margin-top: 0; margin-bottom: 1.2rem; font-size: 0.875rem; font-weight: 600; letter-spacing: 0.015625rem; color: rgba(20, 20, 20, 0.85);">Definition

Code refactoring is the process of restructuring existing code without changing its functionality, in order to improve the design, structure and implementation of the program.
</p>
      
      <p class="body__text" style="margin-top: 0; margin-bottom: 1.2rem; font-size: 0.875rem; font-weight: 600; letter-spacing: 0.015625rem; color: rgba(20, 20, 20, 0.85);">Refactoring ultimately makes software easier to understand, improves performance and helps us find bugs faster.</p>
    </div>
  
    <div class="email__footer" style="padding-right: 3rem; padding-bottom: 2rem; padding-left: 3rem; text-align: center;"><a href="www.techietuts.com" target="_blank">
      <button class="footer__button" type="button" value="Read more" style="display: inline-block; padding: 1rem 3rem; text-transform: uppercase; font-weight: 700; color: #fff; background: #04a257; border: 0; border-radius: 0.125rem; box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23); white-space: nowrap; transition: all 0.25s cubic-bezier(0.4, 0, 1, 1);">Read more</button></a>
    </div>
  </div>

{% endhighlight %}




You may save the file in home/Desktop.


    
<br>
### e) Send the HTML Email

1 . Open the terminal and cd to the location of the html file. Send the file with the following command:

Replace <span class="badge">user_name</span> with your gmail account id.
 
{% highlight terminal %}

mailx -a 'Content-Type: text/html' -s "HTML Template Test" user_name@gmail.com < html_email_example.html

{% endhighlight %}

<br>
The recepient should receive this mail:

<img class="zoom-on-hover mobile-image" srcset="{{site.baseurl}}/assets/img/posts/html_email_gmail.png 2.3x, {{site.baseurl}}/assets/img/posts/html_email_gmail.png 2.3x">


<br>
You could set this up on your server and have your apps automatically send info like news letters, welcome messages when users sign up etc.

<br>
It is worth notting that plain text may be better for personal contact and also remember to give people an option to use plain text when receiving your HTML emails.

<br>
*Thanks for reading, see you in the next one!*
