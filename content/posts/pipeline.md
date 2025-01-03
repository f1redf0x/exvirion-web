+++
title = 'Pipeline'
date = 2025-01-02T18:29:23-07:00
draft = false
tags = ["Cyber-Plumbing"]
categories = ["Dirty-IT"]
+++

# Productivity log:

I'm not going to make this a running thing, but I do need to test out netlify's automated build pipeline and it would be nice to have something in the blogpost other than "TEST" or something remarkably boring like that.

I intended to launch the blog on January first, but I wanted to do so from a fresh Ubuntu machine. I had been meaning to switch for a while from my battered and beaten kali instance. In my view, the following are the only distros I would use:

1. Fedora / rhel - If you trust IBM, want an enterprise grade desktop,  and like dnf
2. Arch - If you want a machine that runs just how you like it and you like pacman and the AUR
3. Debian - If you like apt and stability
4. Mint - If you like apt and flatpaks
5. Ubuntu - If you like apt, snaps, and want an enterprise level desktop
6. Parrot - because you like Hackthebox and want as little friction as possible
7. Kali - because you are are too new or getting too old to customize a pentest rig
8. Alpine - If you like docker or appliance work
9. Openwrt - because Luci is sweet and other routers aren't your style

I wanted an enterprise grade desktop, and quite frankly... I don't trust IBM with Red Hat. After what they did to poor CentOS, it'll be a while before I run rhel on any of my hardware. I would use opensuse because YAST is a treat, but they've been going through a rough time financially and I'd like a desktop with a bit of staying power. (I sure would love to be proven wrong about that though...) 

Given that Ubuntu was the only machine that fit the bill, I decided to migrate my scripts and favorite tools over to Ubuntu. I had a bit of a rough time with it though. That's why I started the blog on day two instead.

# Tar'ring my collectibles:

The first part of any good migration is to make a backup of your stuff. As a former pentester, I'm used to keeping extremely light artifacts, so I could afford the space to just copy my entire home directory into a small file.

```
mkdir HOME_CLONE
cp * HOME_CLONE
cp .* HOME_CLONE
```
Is there a more efficient way to do it? Probably. I bet I could `dd if=/home/user/ of=/home/user/HOME_CLONE` or something, but I'm lazy, and we call dd "disk destroyer" for a reason.

Next, I started going through my .dotfiles to find any legacy stuff. I have a list of programs that are very important to me. If the config files didn't involve the following programs, then I wasn't interested: 

```
kitty pass pass-otp zathura* ranger calcurse taskwarrior timewarrior sxiv lynx vim zsh mpv qutebrowser fetchmail mailutils neomutt htop dino-im yt-dlp openssh-server
```
Note: Your suspicions are correct. I am putting those there despite the security risk of letting a potential adversary know what's on my rig because I want to have a line to paste into future installs thank you very much.

Once I was satisfied that I had stripped out all the garbage, I tar'red the file:
```
tar -czf Home.tar.gz HOME_CLONE
```

Then I secure copied the file to my new machine:
```
scp Home.tar.gz user@machine_ip:~/
```

Then I unzipped the archive file on the new Ubuntu Machine:
```
tar -xzf Home.tar.gz
```

Then I just `mv`'d the files back a directory and `rsync`'d the rest.

# A fun anecdote on tar:
I can't remember who told me the trick for remembering how to tar a file, but it's to imagine a linux admin with a thick german accent.

"Ja, to (C)ompress (Z)e (F)ile, you must tar czf. To e(X)tract (Z)e (F)ile, you must tar xzf." 

Now as far as remembering that the compression, I still need to look that up... but whatever you use on one side, you use on the other.

# The newsboat problem:
My favorite way to interact with the internet is newsboat. If you aren't in my rss feed, you don't exist. It's how I subscribe to youtube, watch reddit,read blogs, catch up with the news if I'm feeling masochistic, etc. So I use snaps to install newsboat and run newsboat ...

...and...

broken.

My themes and settings work quite well, but the issue comes when I want to leave newsboat. I can't launch the vid links with mpv or download them with yt-dlp. I can't use lynx for my 1mb blog brothers. And worst of all, I can't launch qutebrowser so I can render the comics from XKCD, Sara's scribbles, and pizzacake comics! The horror. It turns out that the snap restricts what kind of browsers you could use. I could have used xdg to get the job done, but I wanted my config to just work so I pilfered debian's sid repo, did a `dpkg -i` and called it a day.

# Setting the shell:
So, one of the reasons I want to establish this blog is because there are techy things that I have to do a million times and I'll never dedicate the brain space to remember them. One of those tricks is changing the default shell of the user. Now, if you want to know how to do it, it's easy. It's:
```
sudo chsh -s /usr/bin/zsh -u username
```
But this time around, I did something different. Some user on stackoverflow decided that the best way to fix this problem was to insert the line:
```
exec zsh
```
into their .bashrc file. Spoiler: This was a terrible idea. The .bashrc file is a beautiful config file from a time when systemd wasn't the primary init system. Your system looks for a file that ended in rc and automatically launched it on start. It's great for custom configs. The problem here? I think GNOME (The GUI desktop component) relies somewhat on the the shell being bash. Either that or it just executes zsh and stops moving down the rc file.

The upshot of this stupid move? I can no longer log into my ubuntu machine. I rebooted after making some config changes. I go to the login screen, it goes black, then kicks me out.

Dang...

So, how do we fix this boys and girls? We use one of our other tty's. I pressed ctrl + alt + f3 which took me to the command line on tty 3. I went in, changed the .bashrc to remove the "exec zsh" line, rebooted, and I was good again.

# The fun part about tech:
You know why I'm not afraid of AI taking my job? Because that shell issue set me back hours. Without knowing that the bashrc file was the issue, these were the other things I checked:

1. Back in the early days of gnome, removing the firefox app would uninstall the gui. I thought that removing the snap might have the same effect, So I uninstalled gnome and reinstalled it. Didn't work.
2. Nuked the install and stripped more config files because I thought maybe some config file killed some key component for xorg.
3. Read through the log file for Xorg for 45 mins and googled error messages. Nothing useful.
4. Gave up and used chatgpt to no avail. It didn't get close, because how could it? It didn't know about my bashrc file.
5. Just logged into another tty and used startx so that I could have some kind of gui to play around in.

# My broken dmenu script:
If you aren't using dmenu, rofi, or fzf, I have some news for you... you're about to have a great time on Youtube. Imagine mac's spotlight search or Window's search bar, but ... like ... good. Ok, I'm taking a potshot here, but the ability to write your own menu program is awesome. I used it to create a password manager utility. I already had GNU pass setup with all my passwords, otp codes, and usernames, so I wrote 2 scripts that I mapped to super (window's key), + shift + u or p. If I selected u, it'd give me my vault options, and it would snag the username and put it in my clipboard for pasting. If I hit p, it would determine if it's a password or an otp code and put it in my clipboard. 

Note to self: God, I hope that I don't get clipboard reading malware.

I also wrote another program that uses yt-dlp to grab a list of video titles according to a search term, and launch mpv with my selection.

At least that's what it would have done if it was working at all! The issue, it seemed, was that the dmenu program would launch the menu, get frozen, then refuse to accept any input. If the program was launched by the terminal, it would steal all the keystrokes intended for dmenu. And God help you if the window with all the passwords covered your terminal. 

The issue? I was using wayland, the hot new gui server program instead of xorg, which dmenu relies on.

How the heck did I figure it out? A random github post about dwm freezing in the exact same way as dmenu was. Dwm is part of the same utils package as dmenu, so I figured it makes sense. Did ChatGPT figure that out? NOPE. Score 2 for humans solving human made problems. :) 

# Summary:
One thing I want my blog to be is a reflection of how tech actually is. I hope that this post gave you some useful tidbits like the german zipper or grabbing tty's appropriately, but I also want to show how even if you are doing something as innocuous as moving a couple programs and files over to another linux instance, you should be prepared to understand how the system works because every system will be ready and overly willing to call your bluff. No one can save you from minor mistakes, not even AI. But honestly, that's what I love about computers. If you don't succeed, it's 100% your fault and the machine gives you as many tries as you like (usually).

To quote the guy from P90X: "I hate it, but I love it!"

# Changelog:
Almost as if to prove a point, I tested this post on my pipeline to see if netlify would automatically grab my changes to github. It did, but because I didn't change the Frontmatter draft param from true to false, I kept thinking that netlify made the mistake. Whenever you solve a problem you go through the 5 stages of troubleshooting:

1. Denial - It was netlify!
2. Anger - Why is it doing that?
3. Depression - Ugh, is it going to be another hour?
4. Bargaining - Maybe it just hasn't refreshed the cache yet!
5. Acceptance - Welp, I must have screwed something up...
