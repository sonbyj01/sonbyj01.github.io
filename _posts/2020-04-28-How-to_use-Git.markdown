---
layout: post
title:  "How to use Git(?)"
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

# So where can I use git then? 
Well there are some platforms that are readily available for you to use git with, such as Gitlab and Github. 

-----
![GitlabLogo]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/gitlab.jpg)

-----
![GithubLogo]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/github.png)

-----

# Okay, then what about git commands? What's that?
Git commands are essentially that: they're just commands for git. You can ~~Google~~ DuckDuckGo search these commands but at least for me, most of them didn't make sense unless I saw them being applied. So, that's exactly what I will be doing! 

# How to set up Github
-----
## Setting up an account 
So more often that not, unless you're hosting your own Gitlab server [like me], you're best bet is to use [Github.com]. It's free and a lot of people use it!

-----

##### Create an account
Go to [Github.com] and click "Sign up". Fill in each box with its respective information. Remember: the username you pick now will stick with you forever... and ever...

-----
![Signup]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/signup.PNG)

-----

You'll then be prompted to answer some questions on why you joined Github, afterwhich you'll then be sent a verification email to the address you gave it before. Once you verify your email, you'll be greeted with this page: 

-----
![Github_main]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/github_main.PNG)

-----

You have successfully created an account!

-----

##### Setting up your desktop/laptop
Now the following instructions are for users on Windows. These steps should be fairly similar to those users on Linux and Mac however. 

Okay so now that we've created a Github account, we need something else for our computer to communicate with Github, and that's where [Git] comes into play. Now technically speaking, git is already installed on your computer but (hopefully) you'll see why I like this slightly more. Go ahead and install [Git] onto your computer. After you go through the installation and all, you should have 'Git Bash' when you search for it; open it. 

-----
![gitbash]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/gitbash.PNG)

-----

Now, the beautiful thing about installing from [Git] is that it also installs something called ["MinGW"]. This will essentially give you a 'Unix' feeling, as in most of the basic commands you type in here will also apply to Linux and Mac. Now let's start syncing Github with your computer! Type in the following two commands, replacing 

```bash
git config --global user.name "H Son"       # replace "H Son" with your name
git config --global user.email "testing.sonbyj01@gmail.com"     # replace "testing.sonbyj01@gmail.com" with your email
ssh-keygen -C "testing.sonbyj01@gmail.com" -t rsa       # replace "testing.sonbyj01@gmail.com" with your email
# just click enter for each prompt when doing the 'ssh-keygen'
```

-----
![keygen]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/global_sshkeygen.PNG)

-----

Then type in the following command and copy the output. 

```bash
cat ~/.ssh/id_rsa.pub
```

-----
![sshkey]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/sshkey.PNG)

-----

Then go back to [Github.com]. Click on your profile picture, click "Settings" > "SSH and GPG keys" > "New SSH key".

-----
![settings]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/settings.PNG)

-----

Paste the long string that you copied before and add a name to that key. 

-----
![addingssh]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/addingssh.PNG)

-----

After you save the ssh key, your "SSH and GPG keys" page should look something like this. 

-----
![new_settings]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/new_settings.PNG)

-----

Woo! You finally synced your computer and Github! If you're still having issues, refer to this page on [connecting to Github with SSH]. 

-----

# How to use Github
-----
### Creating your first repository
Now it's time to create your first repository! *I recommend making a repository on Github and then cloning that to your desktop.*

Click on your profile picture > "Your repositories" > "New". You should be on a page similar to this then. 

-----
![create_repo]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/create_repo.PNG)

-----

Add a repository name: this will be the name of your 'project'. You can then either choose 'Public' or 'Private', initialize a 'README', add '.gitignore', and add a 'license'.
* [README] - this is essentially a way for you to articulate the purpose and how-to-use documentation for your repository
* [.gitignore] - this is a file that is inside your repository and in it contains a list of certain files or folders that should **NOT** be uploaded to Github when pushing (i.e. virtual environments, IDE preferences, etc.)
* [license] - there are different types of licenses that you can apply to your repository, the more common ones being MIT License and GNU General Public License v3.0. 

Once you've created your repository, you'll be redirected to that repository's page. 

-----
![repo]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/repo.PNG)

-----

Click on "Clone or download", make sure it says "Clone with SSH", and click the copy button icon right next to the URL. 

-----
![clone]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/clone.PNG)

-----

Then head back to your MINGW terminal. Now a bit of a side node but here are some ['bash commands'] you can use to navigate your way around the terminal. 

```bash
# ls - lists all visable files/folders in your current directory/folder
ls
# cd - moves you into a directory/folder
cd Desktop
# pwd - shows you the absolute path of where you are now
pwd
```

-----
![bash]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/bash.PNG)

-----

Now I'm going to clone my repository onto my Desktop. 

> git clone <repository url (the link you copied before this)> - create a copy of your Github repository to your current location

```bash
cd Desktop
git clone git@github.com:t35t-4cc0unt/Hello-World.git
``` 

-----
![cloning_desktop]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/cloning_desktop.PNG)

-----

If you check on your terminal, you should see your repository name. In my case, it's 'Hello-World'.

-----
![cloning_desktop]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/successful_clone.PNG)

-----

Now enter your repository folder using cd and then open the repository using a code editor (I will be using [VS Code]). If you haven't done so already, download the *free* program.

```bash
cd Hello-World/
code .
```

-----
![opening_code]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/opening_repo.PNG)

-----

If you have VS Code installed, it should have opened with your repository. Now let's doing something pretty simple. I'm going to edit the README.md file and add a small program to this folder. Once I have done that, I will be updating my repository using terminal again. (Note: you can technically do it from VS code, but I'm not going over that right now.)
> git checkout -b <new_branch> - create a new 'branch' on your repository (good practice!) more information on [branches]


> git add <file/folder(s)> - add file/folder(s) that will be pushed to Github (you can also use * to specify all folders and files in that current directory)


> git commit -m '<message>' - add a message to Github that should describe the changes you've made to the file/folder(s)


> git push origin <new_branch> - push the file/folder(s) you added before from your computer (origin) to 'new_branch'

```bash
git checkout -b 'branch1'
git add *
git commit -m "added HelloWorld program and updated README.md"
git push origin branch1
```

-----
![push_new]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/push_new.PNG)

-----

Woo! Congratulations! You've made your first push!

To check it out, go back to your repository page. Click on where it says "Branch: master" (right next to "New pull request") and click on the branch you pushed to. 

-----
![view_branch1]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/view_branch1.PNG)

-----

And boom, there are the changes you've made!

-----
![done_branch1]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/done_branch1.PNG)

-----

### Manage Pull requests
Now notice how there's a yellow, long box and at the end there's a green box labeled "Compare & pull request". Essentially how Github is structured is that there is a 'master' branch that should be working and have the final iteration of changes. It's the 'master' branch for a reason... Now if you want to make some edits to your code, you would branch off of the master and make edits there. That's what we did before when we did 'git checkout -b branch1'. Now, if you're happy with the edits and changes you've made to your code, you can then 'merge' your branch and the master branch, leaving the master branch updated with content from said branch. This is what the "Compare & pull request" is allowing you to do. 

So, click on "Compare & pull request"

-----
![compare_pull1]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/compare_pull1.PNG)

-----

In this upper section, you can see the commit message that you wrote previously when pushing. If you're happy with the edits, click "Create pull request" > then "Merge pull request" > then "Confirm merge". If you scroll down further, you can also see the specific changes that will be made to your files in the master branch.  

-----
![compare_pull2]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/compare_pull2.PNG)

-----

If you also want, on the right side, there's an option to pick either 'Unified' or 'Split'. The split option looks like the one below. 

-----
![compare_pull3]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/compare_pull3.PNG)

-----

Now that you completed the merge request, go back to your repository page and under the master branch you should see your file edits/changes. 

-----

### End of Day Sequence
Now let's say you've been working the entire day, just cranking out code left and right from \*cough stackoverflow and your innovative mind, and it's time to pack up and head back home. Your 'end-of-day' protocol should look something a little like this. 
> git checkout <existing_branch> - switch to an existing branch

```bash
git checkout branch1
git add *
git commit -m "added install script"
git push origin branch1
```

-----
![end-of-day]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/end-of-day.PNG)

-----

I hope this was a resourceful guide on how to use git. And remember, 

-----
![git_meme]({{ site.baseurl }}/assets/images/2020-04-28-How-to_use-Git/git_meme.PNG)

-----

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