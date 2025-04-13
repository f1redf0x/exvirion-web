+++
title = "Quick Tech Tip: There's no place like ~/home"
date = 2025-04-13T11:13:43-05:00
draft = false
tags = ["scripting"]
categories = ["QTT"]
+++
# A Tip for Linux Noobs

This article is going to be part of a series where I cover little tips and tricks that some people don't know, but that make your tech life a little bit easier. Today's tip? 

## Where do I put my scripts?

So when you're working on writing a little bash script, most tutorials will tell you to just make a directory for the project you're working on and just write the script there. That's great for learning, but not good for daily use.

The reason? You have to navigate back to that folder if you want to use your tool. 

Now the way that most people will tell you how to fix that is that you should add that folder to $PATH. $PATH is a variable that tells your terminal where to look for files. When you type "newsboat", it'll look at $PATH and it'll typically look in /usr/local/bin or something like that to find the script. It'll look like this:

```
export PATH="$PATH:~/scripts"
```

and they'll tell you to add it to your .bashrc or something. But there is a better way.

## ~/.local/bin

So whenever linux makes you a home folder, there are a couple of special places in it. One of these special places is called .local. The .local directory contains three files: "bin", "share", and "state". 

bin is for putting scripts. This is where I put all of my tools. If I have a script called "subdomain.sh" in ~/.local/bin, I can type `subdomain.sh` anywhere on my console and $PATH is automatically configured to try ~/.local/bin.

share is for some tools that your system installs. Whenever you `apt install a_tool` it has a decent chance of showing up here. Same if you use python's pip. 

state is a place for tools to store their current state. You won't interact with it much, but if your tools need to record what variables are what, you might want to store files there.

## Why bother?

When you migrate your system, the home folder is the most important because it has all your configs, custom tools, and user data. I find that when I switched to storing all my stuff there instead of in Random custom folders, it became way easier to manage, move, and backup.

So if you want to start doing some shell scripting, try it in ~/.local/bin, then you can ignore all that $PATH manipulation balogna.
