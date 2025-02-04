+++
title = 'Take that, punch clock! Timewarrior'
date = 2025-02-04T12:12:36-06:00
draft = false
tags = ["Timewarrior", "Taskwarrior"]
categories = ["Productivity"]
+++

# The Problem

In my last job, the people were fantastic, the work fascinating, and the clients were decent folks even when things got pretty heated. The biggest problem I had to deal with on a daily basis wasn't anything to do with the normal frustrations of modern working; it was with the Time keeping system!

# Time@IBM

The timekeeping system that my last employer used was awful. It was a website wrapper around some ancient salesforce application. There were a number of thin clients, but the website was the most recent (and most buggy)

The issue with time was it's usage of uuids for client tracking. You had this long string of characters that had NOTHING To do with the client name. You input it, selected a contract tag, and it would add it to your weekly calendar.

These uuids showed up nowhere on this weekly calendar, so the only way you were going to find that uuid (to tell your new colleague on the project) was to rely on a spreadsheet. A freaking spreadsheet! And of course every project had its own and project managers had their own sheets for their own clients. Ugh.

The second biggest issue with the time system was that you often had to count out your hours by going to each and every week on the time application and adding things up manually. We would be asked how many hours we spent on a client between say April 18th - August 9th. 

- We would then go into the week of August 9 and get the numbers
- We would add that to the numbers from August 1st.
- We would add that to numbers from July 22nd.
- and on and on.

We were expected to drop everything to answer these questions which were frequent, becuase the managers didn't have access to run reports that made sense and it can make for huge differences in how budgets are allocated going forward.

In short, we had a time system that couldn't generate reports, couldn't tell you client names, couldn't tell you contract codes, and we had to spend literal hours backtracking through it when there was a minor discrepency.

It SUCKS.

# Introducing taskwarrior and timewarrior

I've been a command line junkie ever since I started using Trusty Tahr back in 2014, and I just hated working with this gui time system. It felt old, archaic, and just like a waste of time. I needed a CLI alternative, and that's where I fell in love with TaskWarrior.

## Taskwarrior

TaskWarrior is a little command line to-do list that is as simple or maddenly complex as you could ever want it to be. It can:

- Give you a list of things you need to do.
- Gives each item a priority score
- Gives you the flexibility to tag items to projects
- Generates reports and burndown charts
- Can limit your tasklist to the top 3 most important things
- And soooo much more.

If you want a really good primer on just how cool this tool is, there is a Series by a Youtuber, named [Bret Martineau](https://www.youtube.com/watch?v=N0IDWHio5qk&list=PLDbCr4bKmB2ll9wBIUlVs4WdXPsA6GYlR). He sets up a fantastic dashboard that rivals Trello or Monday for individual devs

But I'm lazy, so here are the commands that I use:

```
# Adding tasks
task add proj:"project name" "Task title"

# Listing tasks
task

# Marking a task as done
task {id} done

# Deleting a task I never got to.
task {id} delete

# Adding metadata like a uuid to a task/project
task {id} edit

```
And that's all I really use. It tells me what I need to do and keeps track of what I do and when. GREAT NEWS for you people that have annual performance reviews, it remembers every task and project you've done, so you can call those to remembrance.

## TaskWarrior reports

So now you've been working on tasks, and your boss asks you what you've been up to. Now comes the reports. Type the following commands into your terminal:

```
task all
task blocked
task burndown.daily
task burndown.weekly
task burndown monthly
task completed
task summary
```
Those are just a [few](https://taskwarrior.org/docs/report/) of the reports you can run, but screenshot them or copy and paste them into a document for your boss. They can go a long way to show your boss what you are working on.

## TimeWarrior

This is all well and good, but it doesn't solve the original problem. I need to be able to track hours. How much time am I spending on this client or project? 

Yeah, TaskWarrior has you covered there too, but you need an extension. There is a tool called TimeWarrior that monitors how long tasks take. 

```
# To start an assignment
timew start "This assignment" clientname:uuid

# To get assignment IDs
timew summary :week :ids  

# To stop an assignment
timew stop {@id}

# To delete an assignment
timew delete {@id}

# To generate a quick summary report
timew summary

# To summarize all actions taken for a period of time
timew summary 2024-07-18T00:00 - 2024-07-18T00:00:00

# To summarize all work for a client
timew summary clientname:uuid

``` 

## TimeWarrior Hooks

Ok, fine, but now instead of having one inept time system, I have 2 command line ones that I need to keep track of? That's weak...

Well, not quite. Remember how I said that task warrior CAN be complex? You can set hooks in it so that whenever you start a task in TaskWarrior, it'll automatically start the task in TimeWarrior. When you stop a task, it'll automatically be stopped in TimeWarrior. 

All you need to do is use task, and remember how to run `timew summary tag` and you will be good to go. Now if you get asked how many hours you spent on a client, you can get both a TaskWarrior reports breaking down every activity and TimeWarrior reports that declare what hours are spent.

## Hooking up TimeWarrior

A special thanks to Bret Martineau for explaining this process. It's super easy.

```
cd ~/.task/hooks

wget https://raw.githubusercontent.com/GothenburgBitFactory/timewarrior/dev/ext/on-modify.timewarrior

sudo chmod +x on-modify.timewarrior
```
Now that this hook is in place, my new workflow is this:

```
task add proj:clientname "Activity"

task

task start {ID}

task stop {ID}
```

and I have a perfect summary of what I did. And if I want to add notes, then I just edit the task and add them.

# Summary

I didn't include the install instructions because I recommend that you should always get the most up-to-date instructions from the developers themselves, but I've shown you how:

- TaskWarrior is a simple and easy to use to-do list
- How to generate all the stats that managers like to see
- How to track how long you spend on projects
- How to generate reports of how much time you spent on clients
- How to integrate TaskWarrior and TimeWarrior for ease of use.

As a special treat, all of the data in the tasks folder is text. You know what that means... GIT. You can sync your task database to github and then your achievements will live on forever and pushing updates is as simple as:

```
git add .
git status
git commit -m "updating time sheet"
git push
```

TAKE THAT OLD TIME SYSTEM!
