---
layout: post
title:  "How does one use Git"
date:   2020-04-28 00:00:00 -0400
categories: Tutorial
tags: Tutorial Git
description: A double edge sword for programmers...
---
So you're sitting at your computer, working on some intense Tic-Tac-Toe code, when all of a sudden the building you're working on is on fire! What are you going to do? You could 'quickly' open up a Firefox tab, type 'drive.google.com', locate your program, and then drag-and-drop it into Google Drive. 

Or let's saying you're building the next 'Google' website because you have trust issues, but you also know you can't do this alone. How are you and your buddy Gabriel Weinberg going to work on the same code at the same time? 

Oh boy, do I have the perfect candidate for you:

Introducing... Git!

-----

![GitLogo]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/git_logo.gif)

-----

# So what is git? 
Well if you want the textbook definition, here it is:

-----
> Git is a distributed version-control system for tracking changes in source code during software development. It is designed for coordinating work among programmers, but it can be used to track changes in any set of files. Its goals include speed, data integrity, and support for distributed, non-linear workflows.
-----

If none of what was said right before this made sense, just key into these specific parts: *coordinating work among programmers*, *track changes in any set of files*, *speed*. 

# Okay, then what about git commands? What's that?
Git commands are essentially that: they're just commands for git. You can ~~Google~~ DuckDuckGo search these commands but at least for me, most of them didn't make sense unless I saw them being applied. So, that's exactly what I will be doing! 

# Setting up our workstation 
So before we dive into creating repositories and adding some code, let's first download [VS Code] and [Git] (I recommend installing VS Code first before Git). Git is the application that will be driving this entire tutorial while VS Code is a nice, modern editor that we will use to add and edit code. **Note: When installing Git, if you installed VS Code first, you will then be prompted to choose which text editor you want to use during setup. Make sure you choose VS Code!**

# How to use git locally 
Now before we send you out into the wilderness of Github and Gitlab, we must first teach you the basics of git commands and how to navigate your terminal. 
```bash
#######################
## Terminal Commands ##
#######################
# ls - lists all visable files/directories in your current directory
ls
# cd - moves you into a directory
cd Documents
# pwd - shows you the absolute path of where you are now
pwd
# mkdir - makes a new directory 
mkdir NewDirectory
# code <location> - open VS Code in this <location>
code .
# <ctrl> + L - clear the terminal page 

##################
## Git Commands ##
##################
# git init <directory> - will create a new directory and add a .git folder to it
git init HelloWorld
# git add <files> - adds the files you specify or use '--all' to add all the files
git add --all
# git commit -m <message> - write a short message to describe the changes you've made
git commit 'updated README.md and added HelloWorld.cpp'
```

Now that you've seen most of the commands you'll be using in this lesson, let's start applying them!



Thanks for reading - 

sonbyj01

[like me]: https://git.sonbyj01.xyz/
[Github.com]: https://github.com/
[Git]: https://git-scm.com/
["MinGW"]: http://mingw.org/
[connecting to Github with SSH]: https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh
[README]: https://www.makeareadme.com/
[.gitignore]: https://git-scm.com/docs/gitignore
[license]: https://choosealicense.com/
['bash commands']: https://dev.to/awwsmm/101-bash-commands-and-tips-for-beginners-to-experts-30je#pwd-ls-cd
[VS Code]: https://code.visualstudio.com/
[branches]: https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging