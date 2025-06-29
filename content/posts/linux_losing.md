+++
title = 'Linux Is Losing An Evangelist Today'
date = 2025-06-29T14:26:21-05:00
draft = false
tags = ["Linux", "Windows"]
categories = ["Technology"]
+++
Another day, another piece of drama, eh? 

If you haven't heard, the Linux project is going through another dramatic fight. This time it's over X11. For those of you with lives who don't care all that much about computers, X11 is a display server. it's the piece of software that draws all the windows that you use. On Linux, there are 2 options. X11 is the old version that's been rocking since the 80's and Wayland, which is the new kid on the block. 

The controversy is that the team that maintains X11 is stacked with IBM/Redhat employees who want to kill X11 so they can focus on Wayland. A developer who has been pushing patches for years didn't see any progress, so he forked it and now Redhat has made it a political issue. The developer has been slandered as being a conspiracy freak and needs to be taken down, so despite the fact that Wayland isn't a drop in replacement, we as a community need to stand together to ...check notes... kill a software project? 

Son of a b*tch... I'm tired of these children.

This controversy is, for me... the last straw, in a long line of straws. I'm done with Linux advocacy at this point. The blue haired maniacs who run all of the Linux teams have officially killed my interest in these projects. They have forgotten what made Linux special in the first place. Allow me to fill them in on my way out the door.

# My love Of Linux is a Middle Finger to Windows

Growing up, my computing experience was based almost entirely on Windows. My home computer was a Windows Vista machine. At School, we used Windows 7. In church, we'd do geneology on a Windows 10 machine. And my first coding class was done on a Windows 10 machine as well.

I didn't know anything different than Windows, but I noticed some things that started off as annoyances and gradually became deal-breakers.

* The resource usage - My hardware was always a bit on the old side. I'm not a gamer, so the need for a fancy graphics card never really played into my experience. 4 GB of RAM should be plenty... and yet? Windows 10 would often crash on me. Why? Because 2 GB was taken to run all sorts of Microsoft nonsense. Why do you need 2 GB to run Bliss and Defender? Add Chrome on top of that and it would lag so hard a click could take 10 seconds to run. Unacceptable.

* Cortana - Microsoft felt the need to insert software into your experience that you didn't need. Cortana was the first attempt at an AI companion and it was Microsoft's big focus for a while. It was running as NT System/Authority, and had some kind of protection on it that prevented you from deleting it. I played with a version of Windows 10 called LTSC that didn't have it and it ran so smoothly on my hardware. I couldn't understand why I didn't have the right to take it off. And even when I changed its binary's name, Microsoft would change it back. It made me realize that I was renting my OS, not administering it.

* Candy Crush - This was the last straw for me. I ran the professional version of Windows... so why did they put ads in my start menu? 

If you notice, my complaints about Windows are essentially about one concept... Control. It's my computer, so why did big bad Microsoft feel so strongly that they get to dictate my experience?

# Why I switched to Linux

While I was going through it with Windows 10, I got a more powerful laptop for my college courses. It had 8 GB of RAM and I could now officially virtualize things. At the same time, I suffered a real blow to my future plans. I was planning on becoming a mechanical engineer, but one of my professors officially killed any interest I had in the field (by showing me the equation that creates the waveform of the audio from a bullet shrimp punching glass underwater) and telling me that his goal was to turn me into a mathematician. I like math, but not that much.

The combination of a more powerful machine and an aimlessness from losing my way meant that I spent a lot of time on the internet. While on Youtube, I learned about a video game called Watch_Dogs that explored hacking and its effect on the world. I was fascinated. It captured my focus and sent me down a rabbit hole. Part of my research taught me about Linux and how hackers used it to break into systems. I learned that Ubuntu was the easiest form of Linux to use and so I created a virtual machine with the idea that I'd learn what it was and uninstall it.

When I tell you I was immediately in love, I'm not lying. I wiped Windows off my machine that day. Ubuntu's Unity desktop was sleek, creative, and low impact. Applications that were barely functional on my machine were so performant on Linux. And if I didn't want something to be there, I could just uninstall it.

You really have no idea how big a deal that was to me. Amazon had made a deal with Canonical to put a search util in the main bar. It rubbed Linux nerds the wrong way, but coming from Windows? One terminal command and it was gone. What I would have done to have that privilege on Windows... I've spent literal hours into writing debloat scripts for it. With Linux, you have a clean and minimal install right from the beginning. It was amazing.

# The Promise of Linux

From that point one, I was voracious. I would read every article I could. I'd dive into forums. This became a hobby as much as a profession. My first job out of college was in cybersecurity, and I largely believe that it was my Linux skills that set me apart from my peers. The community was rich, opinionated, and fun. Underneath it was the same undercurrent that brought me. It was a promise that I stuck to:

"When you use Linux, it's YOUR computer. Install what YOU want. Use it the way YOU want to."

You want to delete your entire hard drive with a single command? DO IT.
You want to theme the entire thing to be about Hannah Montana? GO FOR IT.
You want to light your icons on fire and turn your desktop into a floating cube. COMPIZ FOR THE WIN.

# But the Community has lost its way

I've noticed a troubling trend in how people treat other users in the Linux space now. When I joined, the excitement was palpable. 

You want to create a live drive that kicks off a laser shooting toaster that hums the Doom theme? 

A bunch of nerds would get together and figure out how to do it.

Nowadays, there is a deep... exhaustion? I don't know, but if you were to take that same example, I promise that the response nowadays would be:

"Why the h*ll would you want to do that?"

Are you high? THAT'S WHY PEOPLE USE LINUX. TO DO THE WEIRD STUFF THAT NO ONE WANTS THEM TO BE ABLE TO DO.

# The Straws

So now you know my story. I came from Windows controlling everything I did, and entered the Linux userspace during a time when everyone was doing cool, weird, and different stuff on their computers. I started to notice people getting less and less creative and more dictatorial on the "right way to use a computer". Now, let me tell you why I'm done with Linux.

1. Ifconfig - There are a number of command line tools that you use a lot. ls, mv, cat, etc. The default tool that you use for interfacing with your networking stack was called ifconfig. It was simple to use. It's what I learned on. But the distros that everyone uses decided to switch to the IP command. There are ok reasons for this, but one day the package maintainers decided that ifconfig and other net tools had to go. And when they got rid of ifconfig, they got rid of route, arp, netstat, and others. This was my first taste of distros deciding that their opinions were more important than user preference. I ignored it... because I could still install the originals, but it did rub me the wrong way.

2. The interfaces - Back when I started playing with linux, network interfaces were simple. eth0 was for the first ethernet interface. wlan0 was for the first wireless interface. It was easy. It was great. The problem was that it wasn't the most predictable scheme. So the nerds came up with a new method. Use information from the firmware to encode the interface's name. eth0 became enp2s0 or enx78e7d1ea46da. All my networking scripts broke over night. I had 3 more steps to perform when doing basic tasks. I assumed this was just Ubuntu being Ubuntu (a theme in my experience), but it moved from Ubuntu to Debian and now I'm stuck with it. 

3. Snapd - Package management in Linux is kind of a mess to be honest. The solution is easy. It's App Images. But unfortunately, Red Hat and Canonical wanted to weigh in so that they could control the future of package management. Red Hat pushed Flatpaks and Ubuntu pushed Snaps. Now, these formats don't really bother me much, other than the fact that they clog up the output of lsblk. But where Ubuntu did me wrong is that they kept removing apt packages that debian had in their repos and replaced them with snaps. If you recall my first article, I mentioned that the move to snaps broke all my RSS tooling and I had to bypass snaps as a result. At this point I started to get reeeeaaaaal Microsoft vibes from Canonical.

4. Systemd - I started googling about snaps and learned that people who typically didn't like snaps were also emphatic haters of systemd. It's just an init program, and I've never used Linux without it, so I didn't know what they were on about, but I did see a bit of the same angst I was beginning to feel. Some nerd somewhere felt that they knew better than you what should run on your computer, Ubuntu and Redhat would embrace the tech, and the ability to use the original tech was phased out. Even if I like systemd, I was on the side of the Devuan and Artix fans who just wanted to be left alone.

5. KEEPASS- Keepass is a password manager for Linux. There are a million derivatives and whatnot, but the ubiquity of it ensured things like browser plugins to be created. Firefox had a wonderful plugin that helped you use one of the Keepass clones. But of course, some developer decided that he didn't want that plugin, so intentionally packaged a version of Keepass that didn't work with it. Another instance of someone making a call that determined what could run on my system. At this point, I started using pass and writing my own dmenu scripts for it. I'm getting so tired of this sh*t.

6. Netplan - D*MMIT, ANOTHER NETWORKING ISSUE? Ubuntu decided that instead of /etc/network/interfaces, it would be better to create another YAML based devops tool called netplan. They screwed with GNOME so that it used this new slop. Once again, they deprecated ifconfig's ability to do its job, and now I have to rewrite all my networking scripts to accomodate it. 

7. Sudo - Oh here we go... the Rustaceans (Another group of opinionated *ssholes who think they have a right to put stuff on my computer) have rewritten a bunch of the core-utils in rust. Now they want to replace sudo with rust-sudo. It's of course missing features, and it's in beta, but it's rust so it must be better. The sad thing here is, I think rust is a good language, but I hate their community's attitude so emphatically that I have a knee-jerk reaction to hearing about their projects.

8. Xorg - And here we are back in the modern day. A bunch of idiots from Red Hat decided that they know better than I do how my computer should run so they are working overtime to kill Xorg. 

# Why it's my last straw

Hey *ssholes... my setup is based on Suckless Software. DWM, DMENU, ST, etc. 

For those of you who don't know, the suckless project is a design philosophy as well as a software repo. They give you all the programs written in C. The configs are managed through the header files. The end result is that you have a lean, mean binary that does exactly what you want and no more.

DWM is a window manager (a program that helps you manage the windows your GUI software draws) that runs on X11. If they did something like unceremoniously take X11 out of the software repos, then my setup would no longer port to new versions of the Operating System. They are breaking my stuff simply to spite a developer that they disagree with.

Oh, and would you look at that... GNOME (one of the big desktop environments with heavy ties to Canonical and Redhat) withdrew support for it and now Ubuntu will be dropping X11 support in their next release. (The same release where they are replacing a bunch of utilities with garbage rust clones).

# Old Man yells at clouds

I get that this rant seems perhaps overblown, and perhaps it is. Technology changes, software packages update, APIs get rewritten. Whatever...

But my issue is, the promise I got when I switched from Windows to Linux is that I was the master of my machine, and the problem is that I now know how patently untrue that is.

If I decide to use a distro, I'm beholden to the package maintainer. If they have some political stick up their *ss about why rust is better, I have to just deal with it. If Canonical decides to remove access to the terminal because it's "better for security", I'll have to deal. If the team that works on openssh happens to mention Bryan Lunduke in an article, SSH will be "considered deprecated" so fast it'll be gone by the next release. 

I might as well just go back to Windows, because I get just as much say about what should be on my system.

# A Pre-emptive Argument 

I've been around the block enough to know that the response to this article will be along the lines of:

"If you don't like what the distro maintainers are doing just leave."

or

"Just fork it an make it your own."

The problem with this brainrot is that it's the same argument people use with surveillance. 

"If you don't pay for the product, you're the product. Just use a product that respects your privacy."

No. Even if you pay for the product, you are for sure being sold for data. Actually, ESPECIALLY if you pay for the product, because they'll want to figure out how to make you pay more. And when one company gets away with it, they all jump onboard. It's why video games all cost 80$ now, it's why every appliance needs to have an app now, and why websites are starting to ask for your driver's licenses.

You know who doesn't have to worry about being spied on? Pirates. Only the bad guys get to have good experiences. Is that really the world you want to live in?

But back to the point... Saying that you can go somewhere else if you aren't happy denies the reality that large players sway everyone else.

Debian is my distro of choice. But if Ubuntu makes a big enough change... it will roll back into debian. Even if I go to Devuan, all of the changes from Ubuntu will make it into Debian, which will enter downstream into Devuan.

These issues aren't bugs of some malignant distro, they are features of a community that has rot in its roots. At this point, trying to tell Linux people that I prefer Xorg to Wayland is like trying to tell Windows that I don't want Cortana. It's pointless, annoying, and it kills the joy I have with using a computer.

So in short... [F*** this s*** I'm out.](https://www.youtube.com/watch?v=I5A7a9FV6H0)

# P.S.

I'll probably talk about this more in another article, but it stands to be mentioned here too. I am a Democrat. I've voted for Kamala Harris, Joe Biden, Hillary Clinton, and I would have voted for Obama if I had been old enough at the time. 

With a voting record like that, you'd assume that I'd be all about deplatforming conservatives and sticking it to the fascists, or whatever slop idiots on my side of the aisle say.

But you'd be wrong. My party has consistently shown itself to be just as sore in victory as they are in defeat and I'm done.

I think that until we clean out all radically open, pseudo-inclusive Liberal types from every software project, the state of computing is going to decline. Good projects are going to die, friendships are going to fracture over meaningless opinions, and the internet is going to be divisive to the point of technological divorce until Conway's law takes over and the undersea cables that connect us to the rest of the world are going to be cut by our governments. And we will cheer for it.
