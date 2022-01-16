---
layout: post
title:  "How to Reset a Forgotten Windows User Password"
author: Denis Kobare
date:   2022-01-16 15:30:00 +0300
img: /assets/img/svg/windows.svg
categories: more-topics
sub_category: operating-systems
type: windows
technology: Windows
permalink: "category/:categories/operating-systems/windows/:year:month/:title"
---

### Introduction

It's rather uncommon to forget your windows login password but sometimes you may find yourself in that situation. If you ever find yourself locked out of your own computer, then worry not because you can actually hack into it and reset the password.


<br>
### Procedure

1 . Power up your computer and at the first appearance of the logo, force it shutdown by pressing and holding the power button.
 
  <img srcset="/assets/img/posts/windows_loading.png 1x, /assets/img/posts/windows_loading.png 2.2x">

<br>
2 . Incessantly repeat step 1 above until system restore starts. You will see the message "Diagnosing your PC"  followed by "Preparing automatic repair"
    <img srcset="/assets/img/posts/windows_preparing.png 1x, /assets/img/posts/windows_preparing.png 1.6x">
    
<br>
3 . Click on "Advanced options"
    <img srcset="/assets/img/posts/automatic_repair_advanced_options.jpg 1.17x, /assets/img/posts/automatic_repair_advanced_options.jpg 2.6x">

<br>
4 . Next click on "Troubleshoot"
    <img srcset="/assets/img/posts/troubleshoot.jpg 1.17x, /assets/img/posts/troubleshoot.jpg 2.6x">

<br>
5 . Next click on "Advanced Options"
    <img srcset="/assets/img/posts/troubleshoot_advanced_options.jpg 1.17x, /assets/img/posts/troubleshoot_advanced_options.jpg 2.6x">

<br>
6 . Next click on "Command Prompt"
    <img srcset="/assets/img/posts/command_prompt.jpg 1x, /assets/img/posts/command_prompt.jpg 2.2x">


<br>
7 . On the command prompt type "regedit" and click enter to open the Registry Editor.
    <img srcset="/assets/img/posts/command_prompt_regedit.png 1x, /assets/img/posts/command_prompt_regedit.png 2x">


<br>
8 . In the Registry Editor, click on the directory "HKEY_LOCAL_MACHINE" to make it the current selection and then click on "File" on the top left corner.
    <img srcset="/assets/img/posts/hkey_local_machine.png 1.16x, /assets/img/posts/hkey_local_machine.png 2.6x">

<br>
9 . Click on "load hive"
    <br><img srcset="/assets/img/posts/load_hive.png 1.16x, /assets/img/posts/load_hive.png 1x">

<br>
10 . Find System32 Directory
    <img srcset="/assets/img/posts/windows_system32.png 1.16x, /assets/img/posts/windows_system32.png 2x">

<br>
11 . Find the file "SYSTEM" in config directory and double click on it.
    <img srcset="/assets/img/posts/config_system.jpg 1.16x, /assets/img/posts/config_system.jpg 2x">

<br>
12 . Type a key name in the load hive prompt. This will be the name of a folder that we're going to use in the next step. Click "Ok"
    <img srcset="/assets/img/posts/key_name.jpg 1.8x, /assets/img/posts/key_name.jpg 4x">

<br>
13 . Open the folder we've just created, find the sub-folder "Setup", click on it and find the file "CmdLine"
    <img srcset="/assets/img/posts/setup_cmdline.jpg 1.8x, /assets/img/posts/setup_cmdline.jpg 4x">

<br>
14 . Double click on the file "CmdLine" and in the window that opens type "cmd.exe" in the second input field and click "Ok"
    <img srcset="/assets/img/posts/cmd_line.jpg 1.8x, /assets/img/posts/cmd_line.jpg 4x">

<br>
15 . Double click on the file "SetupType" and in the window that opens type "2" in the second input field and click "Ok"
    <img srcset="/assets/img/posts/setup_type.jpg 1.8x, /assets/img/posts/setup_type.jpg 4x">

<br>
16 . Click on the directory we created (1234) to make it the current selection and then click on "File" on the top left corner and click "Unload Hive" followed by "Yes".
    <img srcset="/assets/img/posts/select_folder.jpg 1.8x, /assets/img/posts/select_folder.png 4x"> <br><br>
    <img srcset="/assets/img/posts/unload_hive.jpg 1.8x, /assets/img/posts/unload_hive.png 4x">

17 . Close all the windows including the command line


<br>
18 . Click on "Continue". The system will automatically restart.
    <img srcset="/assets/img/posts/continue.jpg 1.8x, /assets/img/posts/continue.jpg 4x">

19 . Upon restarting, the command line will open. Type "net user" to list the user accounts on your machine.
     Type "net user [the_account_name] * ". (NB: If your account name contains several separate words, enclose the account name in quotation marks)
    <img srcset="/assets/img/posts/change_account_password_cmd.jpg 1.4x, /assets/img/posts/change_account_password_cmd.jpg 4x">

20 . Follow the password reset prompts and either leave it blank or input a new password then close the command prompt by typing "exit" followed by pressing enter key. The system will restart and now you are able to login.

<br>
*Thanks for reading, see you in the next one!*
