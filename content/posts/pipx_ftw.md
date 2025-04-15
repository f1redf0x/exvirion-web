+++
title = 'Quick Tech Tip: Pipx FTW'
date = 2025-04-14T15:15:12-05:00
draft = false
tags = ["Python", "HouseKeeping"]
categories = ["QTT"]
+++

# A Tip for Python Noobs

Hello friends, this article is the next in what is sure to be a long line of quick tech tip posts. In this one, we're going to talk about one of python's biggest pain points and how to gracefully deal with it.

## Python's biggest problem

In the deeply opinionated development landscape, you're sure to find conflicting advice about what makes a programming language good or bad. I'm not interested in that. What I am interested in is why my programs keep breaking even though the script just worked last week.

Python has a very active community that is constantly developing new tools, new libraries, and new editions of the language. This rapid pace of development is awesome because it means you can build practically anything really fast, but it comes with a HORRIBLE downside.

## Incompatibility and Dependency Hell

So you have a script that relies on a library from another developer's toolkit. Let's just say openpyxl since I've worked with it before. You have two scripts that rely on openpyxl. 

You need a new feature from openpyxl, so you update your stuff. Your new, third script works fine. The second script doesn't though. So you go and fix the second. 

This works well for a while, but then a coworker who uses script one can't use your script because one of its dependencies was taken down off of a package repo.

Then you have a hard time with a fourth script because a library that might be helpful needs to be built by wheel, but wheel is trying to pull in old scripts that are no longer maintained or missing and the wheel can't build your dependency. So you troubleshoot that for hours, only to find out that one of the packages has been renamed. 

It's a PAIN to deal with python versioning and dependencies.

Side note: I recommend forking or keeping a repo of any project you rely on as mission critical because you never know when it's going to go away.

Side note 2: *expletive* you, wheel. I have more problems with you than successes. I'll never use you to write code, and I hope you eventually rot in some repo somewhere.

Then, there is the biggest problem here. Assuming you can get a copy of all of the dependencies (spoiler, you can't always), you then have to worry about having THE RIGHT COPIES for each program. So if you have openpyxl, the first script might need 2.1.0 and the second might need version 3.1.5. But they share the same name, so when you import openpyxl, it'll default to the later one or something.

## Virtual Environments

The way you fix this absolute mess is to create a virtual environment. A virtual environment is an install of python that's dedicated to a project.

They are easy to setup. You just need to use venv to install a new python interpreter and tell your shell to use it.

```
python3 -m venv projectname
source projectname/bin/activate
```

You heard that right; you need to install python for each project you work on. When you update, install, or import dependencies, you install it to the virtual environment instead of the main OS python interpreter. This gives you the ability to save required versions with tools like `python3 -m pip freeze > requirements.txt`. 

## This isn't Optional

Although borking your system was a privilege you were allowed for a while, OS maintainers have taken that away. In order to simplify maintaining critical OS python packages, OSes like Debian have added the most common python packages to their package managers. If you try to use pip to install a dependency, the OS will not let you unless you are in a virtual environment. To reiterate, Debian won't let you use pip without a virtual environment or a workaround.

You might say to yourself that this seems like a real headache to manage a bunch of different virtual environments. It is, but it's not nearly as bad as:

1. Dealing with conflicting dependencies like you saw earlier.

2. Risk installing a conflicting package to your OS version of python which can break system tools (Looking at you OSX!)

Still, what if you get a tool to the point where you use it daily? You don't want to activate the virtual environment just to use it, then switch back to the default interpreter.

## PIPX, my favorite package manager

[Pipx](https://github.com/pypa/pipx) is a tool that abstracts the virtual environment nonsense out of the way for you. Remember in my last article where I talked about ~/.local? Pipx stores all of these virtual environments and tools in the share folder.

When you install a package with pipx, it'll do the virtual environment stuff on the backend, but register the command to your shell like any other tool. All the benefits of a simple install, but all the dependency benefits of virtual environments.

```
pipx install sometool
sometool
```

It works like a charm. You can also use it to install tools right off of github: 

```
pipx install "git+https://github.com/f1redf0x/cheat.git"
``` 

The above is a version of cheat that I keep for myself because the developer who wrote this tool rewrote it in another programming language and isn't maintaining the python one. Instead of relying on someone else to keep cheat alive, I host it on my github and ensure that it's not corrupted by a downstream attack.

Honestly, unless I'm writing something custom and complex, I never use apt or pip anymore for installing python projects. Pipx has become my default package manager. This has the benefits of keeping my OS install clean, my tools isolated, and I don't have to worry about dependency hell anymore.
