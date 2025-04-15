+++
title = 'How Does a Reverse VPN actually work?'
date = 2025-04-14T21:31:28-05:00
draft = false
tags = ["Cyber-Plumbing", "Ethical Hacking"]
categories = ["Gems", "Spy-phone"]
+++

# The Coolest Hacker Trick I've ever learned

When I was just a pitiful script-kiddie, I spent hours watching Hak5 episodes. There was something so welcoming about Darren and Shannon explaining basic Linux skills. [In this hak5 technolust episode, Darren does something that blew my mind.](https://youtu.be/b7qr0laM8kA?si=IRa2nHB6X4nKomPt&t=226) In the link in the last sentence, watch between 3:46 and 7:10 of the video. He describes a scenario where one of the client devices (in this case, a Lan Turtle) that connects to his vpn server is designated as a "vpn gateway" for all the other devices. 

The long and short of it is that any device that's connected to the vpn can hop through that gateway device as if it was sitting on the same network.

What the actual hell?

This is a device that's behind NAT, behind a firewall, and yet it opens the door for any device that is on the same VPN. The possibilities as a pentest dropbox are obvious. If you could master this skill, then you could arbitrarily access any network that you could sneak a gateway device into.

# My Problem

In the video Darren used OpenVPN to implement this feature and although OpenVPN is a solid product, I was more curious about how this worked on a more fundamental level. I knew it had something to do with turning the client into a router of sorts because he shows the routes that are added to the Lan Turtle upon uploading the configuration file, but no matter what Google search terms I used, I couldn't find how client gateways and "Reverse VPNs" actually worked on a network level.

As an aside, I have a philosophy that played into my frustration. If you can't implement some tech principle in Linux, you probably don't understand it enough because you wouldn't understand what the options are in a config file.

That might seem like the inane ramblings of a neckbeard who needs to get a life, but there is a more practical aspect to that doctrine. If a commercial product removes a feature you rely on, you're going to have to remake that feature in Linux or find a way to live without it when your department refuses to pay for another tool. If OpenVPN removed the client Gateway feature, you're up a creek without a paddle.

# Stumbling into the answer

In my SSH article, I mentioned that I was given access to a couple of routers for a number of cloud environments. There was a project I was in where I was instructed to set up a stable wireguard "site-to-site" vpn. 

I followed the documentation and got devices on cloud 1 to talk to devices on cloud 2, but I couldn't get them to talk to the internet, which was required for updates. 

Because of the speed of our operations, I was given the rule that if I couldn't troubleshoot a problem within a specific time frame, I was to give the challenge to a superior hacker and move onto the next issue on the queue.

THIS ATE ME ALIVE.

I kind of take pride in my infrastructure skills. I one time got drunk on a weekend and woke up with a whole hacker lab setup with a kali machine, 2 subnets, and three victim machines. I have no memory of doing that, but that's how much I enjoy playing with Linux machines. 

The fact that I came so close just to give up made me re-evaluate what I did wrong. I ended up watching [this video by my favorite Youtuber](https://www.youtube.com/watch?v=BZvDSFwOjh4). It turned out that the only thing wrong with my original config was that I hadn't set up NAT masquerading in the iptables.

While I was watching the video though, I learned something CRUCIAL:

```
A REVERSE VPN IS AN UNSANCTIONED 
SITE-TO-SITE VPN. 
```

Actually, while I'm here, let me give some wisdom to the youngsters here.

```
Most tech things have multiple names, depending on context.
```

* Encryption software and ransomware are the exact same thing.

* A backdoor and authorized access can be the same thing (SSH keys).

* A DDOS Attack is the same thing as a load test.

# Back to the original issue

So a site-to-site vpn is a setup where one or more of your vpn devices are configured as routers. Remember that earlier, the "client gateway device" was a device on the VPN that acted like a router. After watching LinuxCloudHack's video, it became clear that Darren or someone like him had renamed an existing concept because it was a unique context for it.

So now that I have an example of a master configuring a standard Ubuntu instance to act as a vpn enabled router, I now have all the details I need to create the principles of a "Reverse-VPN Device".

# What you need for a Client Gateway

It's actually quite simple to make your own client gateway. You just need 4 things:

* A VPN connection

* IP Forwarding enabled

* Configured Static Routes

* NAT Masquerading

Incidentally, these are also the same techniques that are used when [turning a linux device into a makeshift router](https://www.baeldung.com/linux/server-router-configure).

So after all this time, it ends up being remarkably simple to setup. There is no need to rely on a commercial product to pull this off at all, though it is a ton easier to use a tool that handles the steps for you.

# An easier way to do this

Since that video came out, I'd argue that tailscale has the absolute easiest implementation of [this technique](https://tailscale.com/kb/1103/exit-nodes?tab=linux). In fact, theirs is such that you can even pull it off using an Apple TV as the exit node. 

# So why did I bother to write this article?

My goal here isn't exactly to teach people how to make a specific reverse vpn device, but rather to explain a couple of principles. Back when I was learning about this, I would have loved if someone would have explained this process for me. 

As an avid Watch_Dogs fan, I was curious how to turn a smartphone into a gateway device the way that Aiden does in the original game. Now I know. I just needed to root an android phone and setup those features.

So in that vein, I think I'm going to show you how to turn an android phone into a Watch_Dogs styled pentesting rig and then convert it into a reverse vpn gateway device. In short, you connect your phone to a wifi network, and anything connected to your VPN server can use your phone as a jumphost for mischief. Legally sanctioned mischief... but mischief nonetheless.
