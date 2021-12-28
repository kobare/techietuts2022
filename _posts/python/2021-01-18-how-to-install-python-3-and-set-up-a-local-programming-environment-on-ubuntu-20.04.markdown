---
layout: post
title:  "How To Install Python 3 and Set Up a Local Programming Environment on Ubuntu 20.04"
author: Denis Kobare
date:   2021-01-18 09:45:15 +0300
img: /assets/img/svg/python_2.svg
categories: programming
sub_category: python
type: setup-and-configurations
technology: python
tutorial_type: How To
permalink: "category/:categories/python/setup-and-configurations/:year:month/:title"
tags:
- python
- setup
- configuration
- ubuntu 20.04
---


Ubuntu 20.04 ships with Python 3 pre-installed. This guide explains how to configure and set up environments for python projects.


    NB: To use Python 3 on the terminal, you have to explicitly use the command "python3"


### 1. Update Your System
<pre class="language-html">
 <code>
  <span class="unselectable">$</span>sudo apt update && sudo apt -y upgrade
  <h6 class="hashed">
   <br>› Hit:1 http://ke.archive.ubuntu.com/ubuntu focal InRelease
   <br>› Get:2 http://ke.archive.ubuntu.com/ubuntu focal-updates InRelease [114 kB]     
   <br>› Get:3 http://security.ubuntu.com/ubuntu focal-security InRelease [109 kB]      
   <br>› Hit:4 http://repo.mysql.com/apt/ubuntu bionic InRelease                        
   <br>› Hit:5 http://dl.winehq.org/wine-builds/ubuntu artful InRelease                 
   <br>› Hit:6 https://dl.yarnpkg.com/debian stable InRelease ...
  </h6>
</code>
</pre>

<br><br>
 

### 2. Confirm Python is Installed
<pre class="language-html">
<code>
<span class="unselectable">$</span>python3 -V
<span class="unselectable">$</span>Python 3.8.5
</code>
</pre><br><br><br>


### 3. Install PIP
PIP is a package manager for Python packages, or modules.
<pre class="language-html">
<code>
<span class="unselectable">$</span>sudo apt install -y python3-pip
 <h3 class="hashed"># With PIP installed, Python packages can be installed with: </h3>
<span class="unselectable">$</span>pip3 install package_name
 <h6 class="hashed"></h6>
</code>
</pre><br><br><br>


### 4. Install Dependencies
<pre class="language-html">
<code>
<span class="unselectable">$</span>sudo apt install build-essential libssl-dev libffi-dev python-dev
</code>
</pre><br><br><br>



### 5. Set Up a Virtual Environment
A virtual environment allows you to manage separate package installations for different projects. Basically, it isolates your project dependencies from the broader context of your local machine, to avoid conflicts.
To achieve that, we'll use a tool called venv. 
<pre class="language-html">
<code>
 <h4 class="hashed">#Install venv</h4>
 <span class="unselectable">$</span>sudo apt install -y python3-venv
 <h4 class="hashed">#Create a directory called python_projects or any other name. This is where you'll keep your python projects</h4>
 <span class="unselectable">$</span>mkdir python_projects 
</code>
</pre><br><br><br>


### 6. Install virtualenvwrapper
Virtualenvwrapper is a wrapper script around the main virtualenv tool. It helps to organize all of your virtual environments in one location, provides methods to help you easily create, delete, and copy environments and also provides a single command to switch between environments.
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
 <h6 class="hashed">#Install virtualenvwrapper</h6>
 <p class="console">pip3 install virtualenvwrapper</p>
 <h6 class="hashed">#Get the exact location of virtualenvwrapper.sh. You will replace the path on line 4 in step 7 below with this path.</h6>
 <p class="console">which virtualenvwrapper.sh</p>
 <h6 class="hashed">/usr/local/bin/virtualenvwrapper.sh</h6><br>
 <h6 class="hashed">#Alternatively you can do</h6>   
 <p class="console">find / -name virtualenvwrapper.sh</p> 
 <h6 class="hashed">/usr/local/bin/virtualenvwrapper.sh</h6>  
</div>
</section><br><br><br>


### 7. Edit  the ~/.bashrc file or the ~/.profile file 
Using that path, add the following four lines to your shell’s startup file. If you’re using the Bash shell, you would place these lines in either the ~/.bashrc file or the ~/.profile file.
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
 <h6 class="hashed">#Open the ~/.bashrc file with gedit</h6>
 <p class="console">gedit ~/.bashrc</p>
</div>
</section><br>

Copy the code below and add it in the bash file.
<button onclick="copyCodeToClipboard()" class="btn btn-warning btn-sm float-right">Copy</button>
<div id="output"></div>

{% highlight bash linenos%}

export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export WORKON_HOME=$HOME/.virtualenvs   # ".virtualenvs" can be whatever name you like
export PROJECT_HOME=$HOME/python_projects      # "python_projects" can be whatever name you like
source /usr/local/bin/virtualenvwrapper.sh


{% endhighlight %}
<br>

<section class="terminal-container terminal-fixed-top">
Reload the startup file<br><br>
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
 <p class="console">source ~/.bashrc</p>
 <h6 class="hashed">#Confirm there is a directory located at $WORKON_HOME that contains all of the virtualenvwrapper data/files</h6>
 <p class="console">echo $WORKON_HOME</p>
 <h6 class="hashed">/home/sys-admin/.virtualenvs</h6>
</div>
</section><br><br>


<section class="terminal-container terminal-fixed-top">
Now, anytime you want to start a new project, you just have to do this:<br><br>
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
 <p class="console">mkvirtualenv my-new-project</p>
 <h6 class="hashed">(my-new-project) $</h6>
</div><br>
This will create and activate a new environment in the directory located at $WORKON_HOME, where all virtualenvwrapper environments are stored.
</section><br>


<section class="terminal-container terminal-fixed-top">
You’ll also now have the shell commands available to you to help you manage the environments:
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
 <h6 class="hashed">#Create a new environment, in the WORKON_HOME.</h6>
 <p class="console">mkvirtualenv ENVNAME</p>
 <h6 class="hashed">#Remove an environment, in the WORKON_HOME.</h6>
 <p class="console">rmvirtualenv ENVNAME </p>
 <h6 class="hashed">#To stop using an environment, you just need to deactivate it like this</h6>
 <p class="console">deactivate</p>
 <h6 class="hashed">#List environments</h6>
 <p class="console">workon</p>
 <h6 class="hashed">#Activate an environment from the list</h6>
 <p class="console">workon environment_name</p>

</div>
</section><br><br><br>


### 8. Create a Simple Text Game and Play 

<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>


<div class="terminal-home">
 <h6 class="hashed">#cd into the python_projects directory we created earlier</h6>
 <p class="console">cd python_projects</p>
 <h6 class="hashed">#Create a virtual environment for our new project</h6>
 <p class="console">mkvirtualenv text_game</p>
 <h6 class="hashed">#Create a file</h6>
 <p class="console">gedit text_game.py</p>
</div>
</section><br><br><br>

 
<pre class="language-javascript">
<code>

import time #Imports a module to add a pause

#Figuring out how users might respond
answer_A = ["A", "a"]
answer_B = ["B", "b"]
answer_C = ["C", "c"]
yes = ["Y", "y", "yes"]
no = ["N", "n", "no"]

#Grabbing objects
sword = 0
flower = 0

required = ("\nUse only A, B, or C\n") #Cutting down on duplication

#The story is broken into sections, starting with "intro"
def intro():
  print ("After a drunken night out with friends, you awaken the "
  "next morning in a thick, dank forest. Head spinning and " 
  "fighting the urge to vomit, you stand and marvel at your new, "
  "unfamiliar setting. The peace quickly fades when you hear a "
  "grotesque sound emitting behind you. A slobbering orc is "
  "running towards you. You will:")
  time.sleep(1)
  print ("""  A. Grab a nearby rock and throw it at the orc
  B. Lie down and wait to be mauled
  C. Run""")
  choice = input(">>> ") #Here is your first choice.
  if choice in answer_A:
    option_rock()
  elif choice in answer_B:
    print ("\nWelp, that was quick. "
    "\n\nYou died!")
  elif choice in answer_C:
    option_run()
  else:
    print (required)
    intro()

def option_rock(): 
  print ("\nThe orc is stunned, but regains control. He begins "
  "running towards you again. Will you:")
  time.sleep(1)
  print ("""  A. Run
  B. Throw another rock
  C. Run towards a nearby cave""")
  choice = input(">>> ")
  if choice in answer_A:
    option_run()
  elif choice in answer_B:
    print ("\nYou decided to throw another rock, as if the first " 
    "rock thrown did much damage. The rock flew well over the "
    "orcs head. You missed. \n\nYou died!")
  elif choice in answer_C:
    option_cave()
  else:
    print (required)
    option_rock()

def option_cave():
  print ("\nYou were hesitant, since the cave was dark and "
  "ominous. Before you fully enter, you notice a shiny sword on "
  "the ground. Do you pick up a sword. Y/N?")
  choice = input(">>> ")
  if choice in yes:
    sword = 1 #adds a sword
  else:
    sword = 0
  print ("\nWhat do you do next?")
  time.sleep(1)
  print ("""  A. Hide in silence
  B. Fight
  C. Run""")
  choice = input(">>> ")
  if choice in answer_A:
    print ("\nReally? You're going to hide in the dark? I think "
    "orcs can see very well in the dark, right? Not sure, but "
    "I'm going with YES, so...\n\nYou died!")
  elif choice in answer_B:
   if sword > 0:
    print ("\nYou laid in wait. The shimmering sword attracted "
    "the orc, which thought you were no match. As he walked "
    "closer and closer, your heart beat rapidly. As the orc "
    "reached out to grab the sword, you pierced the blade into "
    "its chest. \n\nYou survived!")
   else: #If the user didn't grab the sword
     print ("\nYou should have picked up that sword. You're "
     "defenseless. \n\nYou died!")
  elif choice in answer_C:
    print ("As the orc enters the dark cave, you sliently "
    "sneak out. You're several feet away, but the orc turns "
    "around and sees you running.")
    option_run()
  else:
    print (required)
    option_cave()

def option_run():
  print ("\nYou run as quickly as possible, but the orc's "
  "speed is too great. You will:")
  time.sleep(1)
  print ("""  A. Hide behind boulder
  B. Trapped, so you fight
  C. Run towards an abandoned town""")
  choice = input(">>> ")
  if choice in answer_A:
    print ("You're easily spotted. "
    "\n\nYou died!")
  elif choice in answer_B:
    print ("\nYou're no match for an orc. "
    "\n\nYou died!")
  elif choice in answer_C:
    option_town()
  else:
    print (required)
    option_run()
    
def option_town():
  print ("\nWhile frantically running, you notice a rusted "
  "sword lying in the mud. You quickly reach down and grab it, "
  "but miss. You try to calm your heavy breathing as you hide "
  "behind a delapitated building, waiting for the orc to come "
  "charging around the corner. You notice a purple flower "
  "near your foot. Do you pick it up? Y/N")
  choice = input(">>> ")
  if choice in yes:
    flower = 1 #adds a flower
  else:
    flower = 0
  print ("You hear its heavy footsteps and ready yourself for "
  "the impending orc.")
  time.sleep(1)
  if flower > 0:
    print ("\nYou quickly hold out the purple flower, somehow "
    "hoping it will stop the orc. It does! The orc was looking "
    "for love. "
    "\n\nThis got weird, but you survived!")
  else: #If the user didn't grab the sword
     print ("\nMaybe you should have picked up the flower. "
     "\n\nYou died!")

intro()

</code>
</pre>
<br><br>


### 9. Run the Game and Enjoy
<section class="terminal-container terminal-fixed-top">
<header class="terminal">
<span class="button red"></span>
<span class="button yellow"></span>
<span class="button green"></span>
user@local_machine
</header>

<div class="terminal-home">
<p class="console">python text_game.py</p>
</div>
</section><br><br><br>


That's it! You have successfully set up, configured and tested python.

*See you in the next tuts*

