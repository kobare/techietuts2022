---
layout: post
title:  "Ubuntu Terminal Cheat Sheet"
author: Denis Kobare
date:   2022-06-11 07:00:00 +0300
img: /assets/img/svg/ubuntu.svg
categories: more-topics
sub_category: operating-systems
type: ubuntu
technology: crontab
permalink: "category/:categories/operating-systems/ubuntu/:year:month/:title"
---


<br>
1 .  Navigate back to a previous directory.

    $ cd -


<br>
2 .  Navigate to the home directory

    $ cd ~

or:

    $ cd


<br>
3 .  Clear the terminal

    $ clear

or:

    CTRL + L

<br>
4 . Reset the shell

Clears the screen and removes the history, so you cant scroll back.

    $ reset


<br>
5 . Remember a directory and navigate back to it later.
 
    $ pushd /some_directory   # remembers the directory
    
    $ popd    # navigates back to that directory


<br>
6 . Minimize a command line program to the background.

*NB: Dont close the shell since that will terminate the program*

    CTRL + Z
    
bring it back:

    $ fg    # for foreground


<br>
7 . Execute a command that was last executed.

    $ !!

You can prepend another command to it:

    $ sudo !!


<br>
8 . Search Command History

    CTRL + R

Then type the key words you wish to search for and press enter when you find the command or use CTRL + R to scroll through the matching results.   
    

<br>
9 . Show command history.
    
    $ history

Run a command from the history list:
    
    $ !command_number

e.g

    $ !985   
        
    
<br>
10 . Include the date and time on the history command

Temporarily on the current shell session:

    $ HISTTIMEFORMAT="%Y-%m-%d %T "  #deliberately add the space at the end for readability  


Set the variable permanently:

Open .bashrc file and add the variable on it

    $ nano ~/.bashrc

Add this:

    HISTTIMEFORMAT="%Y-%m-%d %T "    
    

<br>
11 . Adjust font size

    CTRL + +     # increase font size

or:

    CTRL + -     # decrease font size


<br>
12 . Backspace everything out

*NB: Also clears the command from the history*
 
    CTRL + U


<br>
13 . Move the cursor to the begining of a line
 
    CTRL + A


<br>
14 . Move the cursor to the end of a line
 
    CTRL + E


<br>
15 . Chain commands
 
    $ command_1; command_2
    
e.g

    $ mkdir ~/example; cd ~/example   

Alternatively:

*NB: With this method, If the first command fails, subsequent commands won't be executed.*

    $ command_1 && command_2
    
e.g

    $  mkdir ~/example && cd ~/example   


<br>
16 . List everything in current directory
 
    $ ls


<br>
17 . List everything in current directory including hidden files
 
    $ ls -a


<br>
18 . List everything in current directory in a long list format showing file attributes
 
    $ ls -l


<br>
19 . List everything in current directory in a long list format showing file attributes and size in readable format
 
    $ ls -lh


<br>
20 . Tail log files
 
    $ tail -f file_name
    
e.g

    $ tail -f production.log    


<br>
21 . Tail log files more efficiently

*NB: less +F reads the whole file, whereas tail -f only reads the end of the file.*

    $ less +F file_name   

To view the whole file, detach with:

    CTRL + C

Re-attach with:

    SHIFT + F
    
Press q to exit   
    
less +F is impractical for very large files. You can, however, run less -n +F, which causes
less to read only the end of the file, at the cost of not displaying line numbers.    

You can make it truncate lines at the screen width with the -S option, whereas 
tail gives you no choice but to display the whole line no matter how long it is.


<br>
22 . Errase file contents without deleting the file
 
    $ truncate -s 0 file_name


<br>
23 . Pipe a command's output into the column command to make the results readable
 
    $ command_1 | column -t

e.g 

    $ mount | column -t


<br>
24 . Cut the line of text from the cursor point to the end
 
    CTRL + K

Undo the the cut:

    CTRL + Y


<br>
25 . Cut the line of text from the cursor point to the begining
 
    CTRL + U

Undo the the cut:

    CTRL + Y


<br>
26 . Cut words backwords
 
    CTRL + W

Undo the the cut:

    CTRL + Y


<br>
27 . Use a file editor to write long commands or script-like commands 

*NB: Set micro, vim etc as your default text editor, since nano is not suitable for this task*

    CTRL + X + E


<br>
28 . Scroll through previous arguments in history

    ALT + .


<br>
29 . Print the path of the working directory

    $ pwd


<br>
30 . Create a directory

    $ mkdir directory_name


<br>
31 . Navigate into a directory

    $ cd directory_name


<br>
32 . Navigate one level out of a directory

    $ cd ..

<br>
33 . Navigate to the root directory

    $ cd /


<br>
34 . Create a file

    $ touch file_name.extension
    
e.g

    $ touch example.txt    


<br>
35 . Show file content

    $ cat file_name


<br>
36 . Rename a file

    $ mv original_file new_file

e.g

    $ mv file1.txt file2.txt


<br>
37 . Copy a file

    $ cp original_file copy_file


<br>
38 . Compress a file

    $ zip zipped_file_name.zip file_name


<br>
39 . Decompress a file

    $ unzip zipped_file_name.zip 


<br>
40 . Delete a file

    $ rm file_name 


<br>
41 . Delete a directory

    $ rmdir directory_name   # only deletes empty directories

or:

    $ rm -r directory_name


<br>
42 . Find file location

    $ which file_name 


<br>
43 . Show current user's name

    $ whoami 


<br>
44 . Show network information

    $ ifconfig 

For wireless info:

    $ iwconfig


<br>
45 . Find a directory by name

    $ find location_path -type d -name "directory_name" 

e.g
    $ find / -type d -name "dendrite" 


<br>
46 . Check server status

    $ systemctl status server_name 

e.g

    $ systemctl status jellyfin 


<br>
47 . Find information like version number(If not possible with normal method):

    $ dpkg --list | grep file_name

e.g

    $ dpkg --list | grep plex


<br>
48 . Check the system information

    $ uname -a


<br>
49 . Display information about available drives.

    $ blkid


<br>
50 . Show a real-time view of running processes

    $ top


<br>
51 . Display the disk space used in the file system

    $ df


<br>
52 . Display the information about USB buses and the devices connected to them

    $ lsusb


<br>
53 . Running a process in the background

Continue to run processes after exiting the terminal

    $ nohup command_name &

*NB: You must note down the ID. You will need it to kill the process.*


Incase you lose the process id:

Find the process by name

    $ ps -ef | grep "name_of_process" 
    
e.g
       
    $ ps -ef | grep "jekyll"

Kill the process by PID

    $ kill id_here

or:

Bring the process to the foreground and then CTRL + C

    $ fg 


<br>
54 . Install app from package manager

    $ sudo apt-get install app_name


<br>
55 . Uninstall app

    $ sudo apt-get remove app_name


<br>
56 . Update packages

    $ sudo apt-get update


<br>
57 . List files with specific extension

    $ ls *.extension

e.g

    ls *.txt    


<br>
58 . Search words in files

    $ grep -i -R 'the_word' search_path

e.g
 
    $ grep -i -R 'users' /home/Desktop


<br>
59 . Restart machine

    $ sudo shutdown -r

or:
  
    $ sudo reboot    
    

<br>
60 . Shutdown machine

    $ sudo shutdown -h now


Shutdown machine after a specific period of time

    $ sudo shutdown -h 10    
    
    
    # Do this to cancel
    
    $ shutdown -c

or:

    $ sudo shutdown -P +10 

    
<br>
*Thanks for reading, see you in the next one!*
