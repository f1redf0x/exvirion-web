+++
title = 'The proper way to protect cloud data'
date = 2025-03-20T13:08:51-05:00
draft = false
tags = ["CyberSecurity", "data"]
categories = ["Security"]
+++

# If You're Not Paying, You're The Product.

I hate the above phrase. My problem with it isn't in its intent, because I agree with the sentiment. We as consumers need to look at the products we use an determine whether there might be some hidden costs to the convenience that companies provide us.

So what's my issue?

```
EVEN IF YOU PAY FOR A SERVICE, YOU ARE STILL A PRODUCT.
```

I've been thinking of a video that I saw about a year ago. It's from consumer-friendly lobbyist and personal hero of mine, Louis Rossmann.

https://www.youtube.com/watch?v=LGwvLB7rhCE

In it, he discusses that a cloud provider service called Vultr, claimed the right to own all of the data that they hosted. Essentially, if you ran an email server on their infrastructure, they had a right to access and sell that data if they wished.

Now that's a nightmare situation I thought of. I should note that this possibility wasn't mentioned in the video and the comments of the video mention that Vultr's terms of service don't contain that language anymore...

BUT THAT DOESN'T MATTER.

# How Cloud Companies could destroy you in an instant.

What is stopping a company like Microsoft, Amazon, Google, Alibaba, Linode, Vultr, or the like from arbitrarily adding language in their contracts that explicitly grant them access to all the files on your cloud instances? All of your intellectual property becomes theirs. 

Wouldn't you be furious if you found out that Amazon trained their Alexa on your top secret pdf about a product you spent millions to develop because they gave their engineers access to all the drives in their data center? 

A lot of people will point out that the government could subpeona a cloud provider for your data, but it's a lot scarier to think that a 13 year old with chatGPT could get access to your details with a query that accessed a training model that scooped up your cloud info.

# How to protect yourself.

The truth is, this isn't new. Google and Apple either have or have tried to scan photos from your phone's attached cloud drive. Hackers and privacy enthusiasts have created tools that encrypt files locally before sending them into the cloud. And that works... but it's got two issues:

1. The government can just decide those tools are illegal, see [Apple's Advanced Data Protection](https://support.apple.com/en-us/122234)

2. The scope of those tools are really for ordinary people that have photos and documents. They aren't great for small business owners and administrators who manage could servers.

So if you are a standard user, just use cryptomator or whatever alternative you want, no problem. But if you are an admin, consider the following options. 

Note: I'm a Linux Lover and I don't use Microsoft for most things because I'm not a fan of their "We know better than you" method of updating and compelling their users. As such, this article focuses on Linux. There are tricks for Windows and Mac, but I will not be covering them in this article

## Option 1: The DropBear Method

If you don't want someone to get access to your data, encryption is just about the best way to do it. If you think that your cloud provider might try to pull something funny, the first trick that you might try is the use "Full Disk Encryption" or FDE. The problem with FDE in the cloud? Unlocking it is a pain. The first trick you can employ is using Luks to lock your drive. This has three advantages: 

1. No one can read your stuff while the disk is locked.

2. You can have multiple users because key slots exist.

3. You can use a keyslot to add a duress password that'll wipe your drive if someone tries to bruteforce it. See [Kali's documentation](https://www.kali.org/blog/nuke-kali-linux-luks/)

The issue is... it's a server in the cloud and you access it remotely. How are you going to unlock the disk? The first method is to use a tool called [Dropbear](https://linuxways.net/debian/how-to-install-dropbear-on-debian-12/). Dropbear is a stripped down ssh server that's meant to run on things like routers that can't support proper ssh servers or a gui. Since dropbear was designed for headless systems, it can be embeded into the system's bootup procedure.

Follow the guide in that link, and you too can set up a single server with an unlock command.

## Option 2: The Clevis Method

The issue with the above method? Let's say you have a couple dozen servers. Do you really want to ssh into all couple hundred of them? I mean, you can write a script, but then you have to babysit it. 

A lot of Linux admins prefer to use a tool called [Clevis](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/security_hardening/configuring-automated-unlocking-of-encrypted-volumes-using-policy-based-decryption_security-hardening#configuring-automated-unlocking-of-encrypted-volumes-using-policy-based-decryption_security-hardening) to get the job done. Although the Redhat documentation will explain it far better than I can, the idea is that you use a hack with luks, dracut, and pam to call out to an http server on boot. This server, called a Tang Server in the docs, will use some tomfoolery to unlock the luks drives.

This means that if there is a power outage, when the machines come back online, they will all self-unlock if they can hit that Tang machine. It's pretty sweet. I set this up at my last job and it works like a champ.

## A different tact

So the above methods are really awesome for protecting the drive from being mounted. I've heard of decommissioned servers being thrown into the dump without being wiped and if that was a backup server with your data on it, this would protect it. And I actually do believe that this is enough to prevent cloud services from running default dragnet styled tools that they would use for siphoning data.

But I'm not content with stopping passive dragnets, let's think deeper.

Something bugs me. What's stopping a cloud company from using your backup snapshots or cloned images to siphon data? Although [this article from the FBI](https://leb.fbi.gov/articles/featured-articles/executing-search-warrants-in-the-cloud) suggests that cloud providers have no way to access your data, this doesn't make sense to me because I've done work with tools like VMWare and Virtualbox which allow you to both clone Virtual machines and migrate them to other environments. 

Although this would be a pain, I've personally performed techniques where I cloned a virtual machine, booted from a forensic tool iso that was mounted to the VM, used a linux system to mount a windows and linux VM, wipe the password, and gained access to a machine I wasn't supposed to have access to. [Think I'm blowing smoke](https://computersecuritystudent.com/cgi-bin/CSS/process_request_v3.pl?HID=1861b020a4e86e32185403f109866cf4&TYPE=SUB)?

What's more is you can take live snapshots and recover to them if something goes wrong. 

Why can't a cloud provider take a clone of a drive, a running snapshot of a decrypted drive, and send that to another server to mount the image to another vm and grab the data? Sound like a lot of work? Not really, this is second year cybersecurity student stuff, and it'd be very much worth the effort to spy on a competitor. 

And that's the complicated way to do it. Most cloud providers have an http endpoint where you can control your machine through the browser. From this [Geeksforgeeks article](https://www.geeksforgeeks.org/aws-management-console/)

```
This option will launch a browser-based shell environment that is pre-authenticated with your console credentials. shell can be used to execute various AWS CLI commands or scripts using the AWS CDK from your browser.
```

It's all the joy of corporate espionage with the ease of digitally watching over your competitor's shoulder without their permission. I picked on AWS for this, but most cloud companies have this http remote control point that can be configured with identity access management. Why not create a ghost IAM user that won't show up in the user's profile to provide access?

## A side note on law enforcement:

It might seem like I have a chip on my shoulder about law enforcement access to stuff because I'm bringing up tricks that would only really be used legally by agencies like the FBI to conduct investigations.

My issue is more the following (with supporting links about cell-phone technology experiencing these issues to explain why I think this way):

1. If law enforcement has a mechanism, that must mean that a backdoor exists. (Like the [ss7 backdoor in phone networks][https://www.theregister.com/2024/04/02/fcc_ss7_security/] 

2. If a backdoor exists, it can at the very least be used legally or illegally by the company who created the back door (Which they may be incentivized to do). [Like when ATT sold data to law-enforcement that they weren't entitled to via warrant](https://www.techspot.com/news/66830-att-reportedly-paid-spy-customers-us-law-enforcement.html)

3. If a backdoor exists, and the company is willing to do shady things with it for profit, [they WILL sell illegal access](https://www.msn.com/en-us/money/companies/sim-swap-crooks-solicit-t-mobile-us-verizon-staff-via-text-to-do-their-dirty-work/ar-BB1lIQPc).

4. And if a backdoor becomes widly known, it will become a target for the most powerful hacking groups the world has ever known. [Like when American Telecoms were infultrated by the Chinese government](https://www.nbcnews.com/tech/security/chinese-hackers-stole-americans-phone-data-8-telecoms-us-officials-say-rcna182942)

Now granted, I just used a bunch of examples covering multiple parts of the cellular tech stack and didn't focus on a singular issue, but that's kind of the point. Cell data is valuable, so in 4 examples, I showed how SS7, database access, sim-swapping, and conventional hacking was used to compromise the stack at every level.

And you don't think the same won't happen to cloud?

As I've said before, law enforcement need to be given access to resources to conduct their investigations, but as cybersecurity professionals, we need to protect ourselves from points 2-4. 

"Trust but verify" is done. We need zero-trust.

## Option 3: The encryptfs method

So, if you're paranoid like me that everyone is out to get your cloud host in one way or another, let's focus on a method of keeping your data safe while the disk in unencrypted, because... if you are running a cloud service, you probably want it to be running all the time.

For data that's sensitive in nature, I recommend you use [The encryptfs tool that comes natively with your OS](https://ubuntuhandbook.org/index.php/2024/05/encrypt-home-ubuntu-24-04/). By default, when you install a linux box, it will usually do so with LUKS, but there was an older method called encryptfs which used File-Based Encryption to scramble individual files instead of the disk. I think there was a performance boost on older systems, but I think the reason this was a thing was for multi-user systems. If a family shared a computer, each user was only entitled to look at their own data, even if someone else in the family had root.

There are two added benefits by going this route:

1. The encryptfs tools that come natively with ubuntu and other OSes works with PAM to deal with encryption/decryption based on login and logout. When you use this method, your data is only accessible when you are logged into that user.

2. You can still run services on the box as long as you're doing so with another user account. Full disk encryption is nice, but it can't run any tools while it's locked, can it?

## Option 4: The Tombs method

So Tombs are the coolest technology that you've likely never heard about. They are simultaneously nothing new, and extremely new. It's a shell script that wraps around existing linux encryption tools. But why I'm excited about them is a series of [advanced features](https://dyne.org/docs/tomb/#advanced-usage) that no one is talking about.

These are sooo cool that I'm just going to directly quote from the docs:

1. "When it is open, a tomb can bind contents inside the user’s $HOME folder using bind-hooks. For instance, .gnupg will only be found inside your $HOME when the tomb opens."

2. "A tomb can be used on a local machine with keys on a server and never stored on the same device: ssh me@dyne.org 'cat my.tomb.key' | tomb open my.tomb -k - the option -k - tells tomb to take the key from stdin."

3. "It is also possible to store a tomb on a cloud service and mount it locally, ensuring remote servers cannot access contents. One can use sshfs for this:
```
sshfs -o allow_root me@dyne.org:/ /mnt/cloud/
tomb open /mnt/cloud/my.tomb -k my.key
```

[This paper](https://www.researchgate.net/publication/262698824_Data_privacy_in_Desktop_as_a_Service) provides a lot of details about using tombs hosted on cloud storage."

4. Tomb also supports deniable key storage using steganography. One can tomb bury and tomb exhume keys to and from JPEG images when the utility steghide is installed. When securing private data, one must never forget where the keys are. It may be easier to remember a picture, as well it may be less suspicious to transport it and exchange it as a file.

5. The command tomb engrave also allows to backup keys on paper by saving them as printable QR codes, to hide it between the pages of a book. To recover an engraved key, one can scan it with any phone and save the resulting plain text file as the tomb key.

So, in short, tomb can be configured as a package that you mount onto one server from another machine2, open it with gpg from a third server, and it will automatically mount folders from within the tomb into the home folder of the user that summoned it and can be configured to run scripts on boot (I'm looking at you udev).

Hypothetically, you can host your home folder for your computer in the cloud with the components in other jurisdictions and in order to hack your stuff, you'll need to compromise 3 boxes in multiple countries at the same time. 

I'll probably end up doing this for fun and writing an article about it. That sounds like something I would do.

# Summary:

So if you are a Linux admin hosting files in the cloud and you're a paranoid nut-job like me, I've given you 4 ways to protect your stuff. You can:

1. Use full disk encryption and unlock each server when you need it with ssh.

2. Use full disk encryption and unlock all the servers at boot with Tang and Clevis.

3. Use file-based, user home folder encryption with encryptfs which will only be vulnerable at login.

4. Use tomb to create a deployable, encrypted payload that can setup and configure a home-like experience and divvies trust among multiple servers so that multiple servers need to be compromised or someone with cloud access gets lucky when they peek with that browser-shell.

