+++
title = 'Removing Temptation with Scripting'
date = 2025-03-21T15:00:58-05:00
draft = false
tags = ["HouseKeeping", "scripting"]
categories = ["Productivity"]
+++

# My big problem:

If it wasn't clear by the blog already, I hate social media websites with a passion. I dislike Twitter and Mastodon equally. Facebook is my nemesis, and Instagram is my foe. That said, I have a soft spot for Youtube. 

Did I say soft spot? I mean, crippling addiction.

I love Youtube. Whereas older generations turn on the tv and zone out, I pop open an incognito tab so that I get a fresh algorithm each time and ride the wave.

The problem is, it's both a fantastic educational resource and a huge, non-productive time sink. I realized that I don't exactly have the level of control to fight it. If I go in with a problem I need help solving, I'll leave after watching a family guy compilation. No good.

# My first solution:

Wanna make any website on the web inaccessible? No need to go too far and put it in a DNS blocker, just turn off javascript for Youtube. The modern web is built on this fragile code called Javascript and if you turn it off, most websites just refuse to function any further. You go to Youtube? It'll just be some phantom video boxes that'll never load.

My primary Youtube browser is Brave. It blocks Youtube ads and a bunch of other nasty internet intrusions. To turn off javascript for youtube:

```
Go to Settings >> Privacy and security >> Site and Shields Settings >> JavaScript >> Not allowed to use JavaScript [Add] >> www.youtube.com, then click add
```

To test out the now thoroughly borked Youtube instance, go to the website and enjoy nothing, because there is nothing left to see.

## Newsboat

So, for the most part, I don't really use the internet like normal people. I use a program called newsboat that uses a technology called RSS to gather all the new videos and podcasts for me to watch into one program. I've then set up newsboat to open all that content via mpv (music / video player) or Brave. The problem is that I can sort of just...

Open youtube with mpv...

Meaning that it doesn't matter if Youtube is blocked, newsboat will work regardless.

Curse my preparedness.

So I decided to use a little hacker trick to prevent myself from looking at newsboat.

### Step 1:

I went into ~/.local/bin and created this program called newsboat_time.sh:

```
#!/bin/bash

# Get the current hour
hour=$(date +%H)

# Check if the current hour is between 18 (6pm) and 23 (11pm)
if [ $hour -ge 18 ] && [ $hour -le 23 ]; then
  # Run newsboat if the time is between 6pm and 11:59pm
  newsboat
else
  # Print a message if it's working hours
  echo "It's working hours. You are not permitted to use newsboat."
fi
```

### Step 2:

I opened ~/.zshrc and inserted this line:

```
alias newsboat='~/.local/bin/newsboat_time.sh'
```

then I sourced it so that the terminal could see it:

```
source ~/.zshrc
```

## What did that do?

The ~/.zshrc file acts as a configuration file for your shell. When you type an "alias", you're telling the terminal to run the command associated with the alias, NOT the command that shares the same name. So when I type newsboat, it doesn't launch newsboat; it will instead launch my newsboat_time script. That script will check the time on my computer. If it's not after work hours, it'll scold me for trying to use newsboat when I really should be working instead. If it's after work hours, it will launch the program for me.

## Why did I do that?

If you are having problems with controlling yourself, using technology to stop addictive behavior can be an effective strategy. Although there are plenty of other ways to solve the problem (many that are WAY better), this little trick might be good if you are a terminal junkie and you are too lazy to change the clock on your system or remove the alias from your .zshrc file.
