+++
title = 'Taisa: How to Emulate TAILS on Android'
date = 2025-01-03T08:28:41-07:00
draft = false
tags = ["Android"]
categories = ["Privacy"]
+++
Over my 20's, I've spent a ton of time researching privacy and security measures utilized by hackers, spies, and criminals. Anyone who spends any amount of time on the subject will hear about an Operating system called Tails. If you haven't, Tails OS is a linux desktop on a usb stick that is meant to forget everything you do. It's in the name: T.A.I.L.S. is an acronym for The Amnesiac, Incognito, Live System. `https://tails.net/about/index.en.html`

* Amnesiac means that it forgets what you do.
* Incognito means that it allows you to surf without surveillance
* Live System means that all the data is in Ram, which gets cleaned out when you shut down the machine. `https://tails.net/contribute/design/memory_erasure/`

When I was younger, I thought that this idea was the bee's knees. It's a distro that you keep on a keychain, that forgets everything you do, and encrypts everything through TOR (a network protocol that's hard to trace because it adds layers of encryption and pivots packets through other countries). What more could you ask for? 

Well, I did have one item on my wish list, but it was never to be. If you look around, there were some whisperings that the Tails maintainers were looking to create an android fork around 2014, but that fell by the wayside. Having Tails on mobile would be great. The smartphone is how most people choose to interact with the internet after all, and in some locations, it's the only choice. So how can we get the awesome Tails features, but on mobile?

# Requirements:
Whenever you work on a tech project of any kind, it's important to set requirements up at the beginning. This will allow you to know when a project is done and allow you to fight scope creep. In this case, I want 3 protections.

* Amnesiac - I want the OS and more importantly, the disk to forget what I did.
* Incognito - I want my traffic to be routed through tor so that my traffic is hard to trace.
* Live System - I'm going to make a compromise here. It doesn't have to be a live system, it just has to protect and clear my RAM.

# A Note on the live system:

I used to play with Custom Roms a lot back in the day, but I was a real script kiddie when I did that. I knew how to unlock the bootloader and use fastboot, but I didn't really understand it. I still don't to this day, and at this point, I don't have the hardware to mess around with that. Also, is it just me or is every phone locking their bootloader unneccessarily? Since there is no guarantee that our corporate overlords will allow us to keep unlocking the devices we paid for, I'm just going to focus on RAM cleaning.

# Incognito first:
Getting incognito on android isn't hard at all. Crack open the app store (or Aurora if you hate google play) and download the tor-browser app. If you want to do the whole system, do orbot and test it out. My particular threat model for my tails instance only calls for browser traffic to be encrypted, so orbot is a bit overkill for what I'm doing. But if you want TAILS, orbot is closer to the end goal.

Now please enjoy this article written by a forensics team that look into the tor browser on android to see what artifacts can be recovered: `https://ieeexplore.ieee.org/document/9568880`. This article is old, so I'm sure things have shifted around, but it's always important to see what forensics experts can see because they are smarter than us mere mortals.

# Amnesiac Second:
Now, sneak peak, I'm going to be choosing grapheneOS as the base for this little idea because their documentation is much clearer than Google's.

Google has a feature for their phones called "user profiles". A user profile is it's own little mounted user container (this is a Ross-made abstraction... I don't actually know how this mechanism works) that contains it's own encryption keys, it's own apps, it's own identifiers, etc. A user profile signals to Google that the user of this container is their own user. 

User profiles come in all sorts of different flavors. One of which is useful to us. It's called a guest profile. A guest profile is really meant for if someone asks you to borrow your phone. You don't want them to have access to all your stuff, but you also want them to have a really locked down sandbox to play in. It lists as one of it's features that you can delete all the data when the guest exits the profile. It's a single time use.

Now, if you're paranoid like me, then the next thought you had when you heard about that was, "Ok, but just because data is deleted, doesn't mean that it's gone. It can be sitting on the disk waiting for another program to overwrite it. How can we trust it?" The answer is encryption. 

The following whitepaper is from Google circa 2021. Although that puts it 4 years out of date, it doesn't seem that much has changed when I look at their documentation website, and some of the language is clearer in this paper: `https://source.android.com/docs/security/overview/reports/Google_Android_Enterprise_Security_Whitepaper_2020.pdf`. I'd encourage you to read it, but this line sticks out for me:

```
"On devices with more than one user, each user has their own 
encryption keys, with CE keys bound to that user; this improves 
on FDE, which has only a single key bound to the first user, 
which unlocks all user data on the device. Encryption keys 
are 256 bits long and generated randomly on-device. Devices 
running Android 9 and higher can use adoptable storage and FBE.
An additional layer of encryption protects information, such 
as directory layouts, file sizes, permissions and creation/modification 
times (collectively this is known as file system metadata)"
```

So even if you don't have the cash for a pixel running graphene, it seems that each user account with a password has it's own unique encryption key. Now since the guest user is purged after every use, my guess is that it's encrypted key that gets unlocked by user password is tossed each time. Why do I feel comfortable guessing that? Devs rely on abstractions and I'm guessing that's how the user profile objects work in general in android, and I don't think they'd add a seperate mechanism for just the guest profile. Even if it doesn't purge the key though, there is a simple solution... just use a longish password. Do the diceware thing where you pick three random words from the dictionary for the password for a guest pofile. Then when you're done, and the guest profile is wiped, forget about it. It was a one time password, why the heck would you remember it? So even if the vault containing the guest key survives (doubtful), you still don't have the password to enter it. 

And this problem involves less guesswork in Graphene os. From their documentation found here `https://grapheneos.org/features#improved-user-profiles`:

```
"Android's user profiles are isolated workspaces with their own 
instances of apps, app data and profile data (contacts, media 
store, home directory, etc.). Apps can't see the apps in other 
user profiles and can only communicate with apps within the 
same user profile (with mutual consent with the other app). 
Each user profile has their own encryption keys based on their 
lock method. They're a great fit for GrapheneOS with a lot of 
room for improvement."

and

"GrapheneOS also enables support for logging out of user profiles
without needing a device manager controlling the device to use 
this feature. Logging out makes profiles inactive so none of the 
apps installed in them can run. It also purges the disk encryption 
keys from memory and hardware registers, putting the user profile 
back at rest."
```

Though the existence of this second quote makes me think that perhaps stock android doesn't purge encryption keys from memory and hardware registers, so the move would be to reboot the device after you've used the guest session.

# Summary so far:

Ok, I know... that's a decent bit of reading, but if you install orbot / tor in a guest profile, You have an amnesiac incognito system that will encrypt everything you do on the disk, obscure everything you do on the network (in line with the limitations of the TOR network), and will erase itself. Now we just need to worry about that pesky RAM issue, because again... just because a computer lets Ram, storage, bits in cpu registers go doesn't mean that they're gone. They still need to be overwritten.

# Cleaning the Ram with Graphene

This is where Graphene comes in clutch again. From their documentation here `https://grapheneos.org/features#clearing-sensitive-data-from-memory`:

```
"As documented in our section on our added exploit mitigations, 
GrapheneOS adds zeroing of freed memory to both the standard 
userspace and kernel allocators. These features have the 
secondary benefit of clearing sensitive data from memory as 
soon as possible in addition to defending against exploits. 
Android implements regular compaction of frozen cached apps 
and apps currently running in the background based on 
triggering a full compacting garbage collection (GC) and then 
requesting that malloc frees as much memory as it can back to 
the OS. This pairs well with zeroing features and results in 
freed data getting cleared faster for Java/Kotlin and also 
the C, C++ and Rust libraries used by them where low-level 
allocators get held onto until the high level objects are freed.

When the device is locked, we trigger full compacting garbage 
collection (GC) for the SystemUI and system_server processes to 
release all of the memory that's no longer used back to the OS. 
Due to GrapheneOS enabling kernel page allocator zeroing, this 
results in all the no longer referenced data in objects being 
cleared. We based our approach on Android's standard approach 
to running a full compacting GC for these two processes after 
the device is unlocked to clear remnants of the user's 
PIN/password and keys derived from it. This is a nice way to 
clear some data immediately after locking prior to our auto-reboot 
feature kicking in to clear all of the OS memory.

GrapheneOS modifies some of the ways the device can be rebooted 
to proceed with the normal reboot process where memory gets freed 
and cleared by the kernel page and slab allocator zeroing features 
enabled by GrapheneOS."
``` 

So to recap, if you host your pseudo-tails platform on Graphene, then every time you lock the device, it'll start zero'ing out the ram. 

# TAISA: Mobile Tails

So now that we've hit our build requirements as best as we can, the following describes the T.A.I.S.A., or The Amnesiac Icognito Secured Account:

1. Start with a graphene os phone.
2. Install via the app store either orbot or the tor-browser.
3. In the settings, enable multi-user.
4. Make sure that delete guest data is selected on the multi-user page.
5. Create a guest account, and port orbot/tor in there.
6. Set it up with a diceware password and no biometrics. This will create a unique encryption key for that account.
7. Do whatever you want, it's just android.
8. Exit the guest mode. This will delete the guest account and PROBABLY the data that the password interacts with to unlock the device.
9. Lock the phone to enable Ram cleaning.

There should now be zero trace of what you did, and a decent forensics expert (a good stand in for criminals with advanced cracking toolkits) will now need to find and probably recover the un-allocated vault that your password unlocked, guess your diceware password, and run forensics on the tor-browser which will probably yield very little of what you did during your session.

# Caveats:

Now nothing is perfect, so I'm thinking of how to exploit this:

1. If your phone has malware or rooted, the primary profile should be able to see what guest is doing.
2. Any services that you hit will have a record that you went there, even if it's a torified record.
3. There are some features that are shared between your main profile and the guest, things like wifi settings, and alarms. For more info on this, see that security whitepaper from earlier and look for the section titled: "Cross Profile Data Sharing".

# The Challenge:

The quickest way to find out that something is incorrect is to post something confidently on the internet. So, I'm going to make a bold claim here. I don't think that someone can break TAISA, minus those caveats above. I think that the security stack of Google, graphene, and the tor project are so strong that even a smarty pants like you can't break it.

Soooo... 

If someone does manage to get a piece of browsing history from a TAISA session, I will print a retraction and hail your name. Otherwise, I'm considering this to be a solved problem.
