+++
title = 'The Horrifyingly Effectiveness Of SSH'
date = 2025-04-08T12:10:28-05:00
draft = false
tags = ["Cyber-Plumbing", "SSH"]
categories = ["Dirty-IT"]
+++

# A Confession

So, in my last job, I was banned from using SSH. I'm only kind of joking when I say that. I was something of a network duct-taper. We had networks in different clouds with bastion servers, and weird firewalls and other restrictions that would come out later in testing.

My Boss was a badass, constantly developing scripts and tools as fast as he could to answer weird edge cases that some big-wig wanted immediately. The problem is that in this environment of "Move Fast and Break Things", setting up a proper network architecture was tricky. We needed proof of concepts that could bridge resources from multiple networks. There were official routers that we were given access to, but with things shifting from one cloud to another so fast, and with the routers being more complex than I could mess with without risking a breach, I would often cheat.

SSH, or Secure SHell is probably the most versatile protocol that I know of. You can use it to access machines in other networks, tunnel traffic, and proxy information. As such, if I could touch a server with SSH enabled, you'd best believe I was using it to accomplish the task.

I've tunnelled Jenkins, Ansible, Teleport, VSCode, Kali Tools, SAML, Docker-based applications, presentations, notebooks (Jupyter and vim), websites, and even entire networks by using tunnels with a gateway device.

In short, my use of SSH was the literal definition for giving me an inch would give me a mile. Although my boss was grateful that I could get him what he needed in minutes, he was often shocked and horrified with the tomfoolery I had used to accomplish my goal. As such, I was banned from using ssh except for very strict use-cases. 

It was one of my finest moments as a grown up.

I've seen tons of articles talking about SSH, but very few people use it like I do, so I decided to open my playbook a little to show you what you are missing. 

## Remote Access:

Starting off, let's go with an easy one. This is the default usecase of SSH. You want to log into another machine:

```
ssh user@ip_address
```

This is boring, but powerful... [unless you're a bit of a monster...](https://null-byte.wonderhowto.com/how-to/haunt-computer-with-ssh-0199625/) All you are getting is access to a remote machine to run commands. 

## Passing Keys

Next, let's talk about passing keys. When you are starting out, you might use a password to log into a box, but very quickly you'll be told to generate a keypair. Here's a move that seperates the tutorial-followers from the ssh-natives: You can tell ssh to install the key onto the box when you login:

```
ssh-copy-id -i id_rsa user@ip_address
```

-i tells ssh to use a specific key file. You type in your password, it'll confirm, and it'll automatically write the public key into the user's authorized key files. No need to do that "Cat the public key to your clipboard and paste it in the other machine." We're hackers. Laziness is the way to go.

## Transferring files

This is another really common thing you'll probably need to do. Most of my old coworkers used VS-Code's file manager to download files to their computer. That's cute, but unstable. If you have a file that's relatively large, that might tank your VS-Code instance. It's better to use ssh to transfer large files. It's easy to do. If you want to push a file from your local computer to a remote computer:

```
scp file.txt user@ip_address:/path/to/new/location
```

If you want to transfer a file back to your computer from the remote machine.

```
scp user@ip_address:/file/you/want .
```

That'll snag the file you want and put it right in the folder that your terminal is in.

The only time I wouldn't use this technique is for when the file is monstrous. Any more than 2 gigs, and it's likely the ssh session might go bad. For that, I'll use rsync since rsync can continue from where it left off.

## Hidden Service

This next one will get you in a ton of trouble if you work for an IT department, but it's a really good trick if you want to access your machine from anywhere in the world. 

There is a service called TOR, which is used to anonymize your traffic. It also punches holes through NAT like no one's business. You can give your SSH server to TOR and you can access the server from anywhere in the world anonymously. I found this trick on a Wiki for an app called [Termux](https://wiki.termux.com/wiki/Bypassing_NAT). These folks were using tor to remote control their phones. What mad lads!

On Debian:

```
sudo apt install tor proxychains-ng
vim /etc/tor/torrc
```
the toorc file needs to look like this:
```
## Enable TOR SOCKS proxy
SOCKSPort 127.0.0.1:9050

## Hidden Service: SSH
HiddenServiceDir /home/.tor/hidden_ssh
HiddenServicePort 22 127.0.0.1:8022
```
Then you need to create a tor service file so that tor knows where to place the hostname:
```
mkdir -p ~/.tor/hidden_ssh
tor
cat ~/.tor/hidden_ssh/hostname #This will give you the onion address.
```

Then you can access your machine by using proxychains on another machine:
```
proxychains4 ssh abunchofrandomcharacters.onion
```
## Creating a backdoor

Hey, have a rubber ducky, bluetooth ducky, nethunter phone, or some other ducky script distribution system and six seconds to act? Write a ducky script that:

1. Open a terminal.
2. Cats a public key into the OS's default ssh authorized key file.

Now you'll have a persistent back door into the machine, particularly if passwords for ssh are disabled. This works on Mac and Linux, and sometimes Windows though SSH on Windows is still relatively rare. 

## Proxying

So, you have access to an ssh box that's inside a network with an internal webserver. It's easy to go to that website. All you need to do is:

```
ssh -L 127.0.0.1:8080:127.0.0.1:8080 user@ip_address
```

Oh My God, did you just see what I did? Notice how there are 2 127.0.0.1 addresses in there? Most tutorials skip over this, but the 8080:localhost:8080 syntax is actually a shorthand for localhost:8080:localhost:8080. If you are just connecting the client to the server, you don't need to write it out. It's important to know though because you can pick ANOTHER IP ON YOUR NETWORK to forward the traffic for. 

Ok, back to what we were doing; Go to your browser of choice and type this in the URL bar:

```
http:127.0.0.1:8080
```

Your browser will connect to the local port setup with ssh, which will take the browser and send it through the ssh server to the web application on the same box.

Now you are cooking with gas. If you care about all the ways you can use ssh to connect to internally facing resources, read this [book and be amazed!](https://github.com/opsdisk/the_cyber_plumbers_handbook)

## Reverse Shell

Ok, so this will come as no surprise to the hackers, but to the rest of you, forget Cloudflare tunnel. If you have a server on Linode or AWS with a public IP, then you can use it as a connection server for your devices. The way a reverse shell is set up is:

1. You tell a device (device1) sitting behind a firewall to call out to the internet. `ssh -R 127.0.0.1:8022:127.0.0.1:22 user@ip_address`

2. The device on the internet (device2) will hold the connection for device1.

3. A third device (device3), like a laptop, will login to device2 using ssh. `ssh user@ip_address`

4. They use ssh on device2 to connect to device1. `ssh -p 8022 user@127.0.0.1`

Now, the important thing to know here is that when you use the -R flag the ports trade places. 

When you use -L, 127.0.0.1:22:127.0.0.1:8022 means connect the port 22 on my current machine to the port 8022 on the remote machine.

When you use -R, 127.0.0.1:22:127.0.0.1:8022 means connect port 8022 on my current machine to the port 22 on the remote machine. It's reversed.

The reason for this comes down to the definition of a "bind socket" in the documentation. It makes more sense if you RTFM, but since you probably won't, just know that everything you know is reversed when you use a reverse shell.

## Tunneling Gui Apps

So let's say you have a Kali instance that's sitting on the Cloud, but you want to use a tool like Wireshark or Burp which are GUI tools. You could setup RDP on the machine and use Remmina or xfreerdp to login to the kali box, but that's lame.

Instead, use X11 forwarding!

```
ssh -X user@ip_address
```

It doesn't feel too different, but from the machine over there, type `wireshark&`. It'll spawn a window on your local instance.

Note: This will require a [modification to your ssh server](https://unix.stackexchange.com/questions/12755/how-to-forward-x-over-ssh-to-run-graphics-applications-remotely). Also, it won't port audio for audio applications. That said, it's possible to [forward audio if you use alsa](https://usercomp.com/news/1063656/play-sounds-over-ssh-complete-guide).

## OSI L3 VPN

Ok, so you want to VPN into a network and access all the services on a network that your ssh server can touch? Easy. Use [SSHuttle!](https://github.com/sshuttle/sshuttle)

`sshuttle --dns -NHr username@sshserver 10.0.0.0/8 172.16.0.0/12 192.168.0.0/16`

This tool is sooo cool. It uses ssh to login to an ssh server, then uses it's python interpreter to relay all traffic back and forth with the network. So if you use the command above to get onto a home network, you'll be able to not only access the ssh server (for example: 192.168.0.5), but all the other boxes as well (192.168.0.1-254). 

I'll admit, this is the one that got me banned from using ssh at work. XD

## OSI L2 VPN

Here is a fun fact, you want to setup a full VPN between a current device and another network? Oh, you want it to use layer 2 so you can mess with arp spoofing and stuff... Congrats! There is an ssh tool for you too:

[Arch Wiki Article on an SSH VPN](https://wiki.archlinux.org/title/VPN_over_SSH)

Now, this is not my ideal way to use a VPN. I prefer wireguard, tailscale, and other tools for this, but ordinarily you need a full bridge for this capability.

## Imitating Github

So this trick is more a team effort with ssh and git, but you can configure ssh to only run one command which might be git. You can also restrict what shell a person uses and what abilities a user doesn't have when logged in. 

[Check out this article from the git team on how to set up a server](https://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server). 

Side note: Why set up Gitea when this is an option?

If you didn't read the page, it shows you how to set up a personal git repo on a linux box you run and shows you how to setup ssh access for your buddies so they can host code on your box without you needing to give them anything else.

# Summary

I haven't even scratched the surface of what you can do with ssh. Some notable exclusions:

1. Create your own proxy network with proxychains and ssh.

2. Jumping from host to host using -J.

3. Unlocking Luks drives with a single command.

4. Setting up agents so that you don't need to decrypt the key when jumping.

5. How to exploit agents and steal other user's ssh sessions.

6. Master mode so that you only need to authenticate once.

7. Setup Kerberos for SSH using GSSAPI.

8. Mount an entire filesystem with sshfs

9. Setting up a Socks proxy for evading internet restrictions

10. Send your microphone to the [server's speaker](https://web.archive.org/web/20241015055433/https://www.blackmoreops.com/2016/11/08/top-30-ssh-shenanigans/) `dd if=/dev/dsp | ssh -c arcfour -C username@host dd of=/dev/dsp` 


