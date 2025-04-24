+++
title = 'How immutability works on Linux'
date = 2025-04-24T14:07:21-05:00
draft = false
tags = ["immutability"]
categories = ["Linux"]
+++

This is gonna be a pretty short article, but hopefully it saves someone some time. 

Ever since [NixOS](https://nixos.org/) hit the scene, there has been a growing movement of linux practitioners that extol the idea of an immutable distribution, which is an OS where the system itself is read-only, and all the applications are installed via containers.

# My Uninformed Opinion

This idea is a bit of a mixed bag for me.

1. On the first hand, I love using live linux distros as "ghost operating systems" where I do my thing and nothing is left behind. It keeps the system stable, uncluttered, and I can't think of a setup that's more private than one that forgets everything you do.

2. On the other hand, this sounds exactly like how android works. There is a read only operating system (known as a ROM) with a specifically mounted filesystem location that can be written to. All your apps are java virtual machines, and the data for these apps are stored in that writable part of the OS.

This is annoying for 2 reasons:

- If the experience using an immutable distro is similar to android, then you're locked into a pretty pathetic jail. All the fun toys like iptables and packet forwarding are left to the system and you can't change them. You need to root your phone to do that and I imagine it's going to be the same in an immutable distro. Hell, if they don't include sudo in the OS's repo you might not get root in some distros.

- You would need to rely on a packaging format like snap or flatpaks in order to deploy apps because package managers like apt and yum install things in /usr/local and /usr bin which are part of the root filesystem. Now as an aside, I really like flatpaks, but I don't like putting blind trust in either Redhat and Canonical. I respect the work of those two companies, but package management is really the sort of thing you don't want to lock people to and give the keys to a corporate entity that values profit over all else. Canonical has been known [to make controversial moves](https://arstechnica.com/information-technology/2012/12/richard-stallman-calls-ubuntu-spyware-because-it-tracks-searches/) and Redhat has made some [questionable](https://itsfoss.com/centos-stream-fiasco/) [decisions](https://lwn.net/Articles/933525/) as well.

Now, annoyances aside, immutability is pretty solid from a security perspective. Upgrades are easier, as are rollbacks. FIM can detect an issue immediately if ANYTHING IS MOUNTED AS RW, and the fact that so much of the OS is frozen means you can do things like secure booting only approved images... which as much as I'll whine about Apple is the sort of thing that makes the iphone so unbelievably hard to compromise.

# My Big Question:

I've thought about setting up a project called a "Shroedinger box" which is a live Linux distro with the ability to update itself and all user data is stored offsite in a tomb with automount hooks, but the immutability thing sounds promising. The question I had to ask though is:

```
How does an immutable system deal
with logs and user data?
```

None of the Youtubers covering the topic, none of the news articles I've seen, and none of the search engines gave me a proper answer to this. 

I decided to dive into the documentation for some of these, and it looks like [Fedora Silverblue](https://docs.fedoraproject.org/en-US/fedora-silverblue/technical-information/) has the answer. From the docs:

```
On Fedora Silverblue, the root filesystem (/) is immutable. The /usr directory and everything below it is read-only.

The /etc and /var directories are respectively used to store configuration files and runtime state and are thus writable. Symlinks are used to make traditional state-carrying directories available in their expected locations. This includes:

/home → /var/home

/opt → /var/opt

/srv → /var/srv

/root → /var/roothome

/usr/local → /var/usrlocal

/mnt→ /var/mnt

/tmp → /sysroot/tmp

This means that separate home partitions should be mounted on /var/home

```

So it looks like there are two mountpoints that are still "mutable". /etc is still there for configuration files, and /var is still there for logs. As far as user data goes, they've structured all of the stragglers to utilize /var as a dumping ground. If you create a new user, it'll go to /var/home and create the user there. That means the user still has autonomy and freedom within their own home folder.

The documentation then points to a tool called [libostree](https://ostreedev.github.io/ostree/adapting-existing/) which seems to be the underlying mechanism for swapping out immutable filesystems. It's a really neat idea to tell you the truth. I might have to play with this to make some kind of a debian schroedinger instance, but I'm a fan of how this is run.

# The Answer

So yeah, immutable distros have logs and user data. they are sequestered to parts of the filesystem that are writable similar to tiny core linux. Curiosity satisfied.
