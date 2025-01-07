+++
title = 'This Phone Will Self Destruct in 24 Hours'
date = 2025-01-07T09:59:14-07:00
draft = false
tags = ["android"]
categories = ["Spy-phone"]
+++

We've all seen that scene in Mission Impossible where a secret agent picks up a device, it relays a message, and then it utters the unforgettable line, "This message will now self destruct". I love these scenes, and I've always wondered why normal civilians can't have devices with cool spycraft features like that. 

Well, on Android, there totally are features like that and I wanted to explain some of them here in case you weren't familiar.

# AN IMPORTANT DISCLAIMER:

Before I begin, I want to address a thin, blue elephant that'll be sitting in the corner throughout, which is law enforcement. When I talk about these techniques and tricks, I use law enforcement as a benchmark because any tool that they have WILL eventually fall in the hands of a criminal. If the cops have a UFED extractor, then the bad guys will have one too. Don't believe me? The following article explains that the drug cartels of Mexico have their own cellular carrier. [https://www.hackers-arise.com/post/mobile-hacking-how-the-mexican-drug-cartels-built-their-own-cellular-infrastructure-to-avoid-survei]. If you can spin up your own telecom network, I think you can get a cellebrite toolkit.
 
I want to make it clear that law enforcement is just doing their job and that needs to be respected. You should always cooperate with police to the fullest extent of the law in your country and jurisdiction.

Now in that vein, I have a job which too needs to be respected which is to protect data from unauthorized intrusion. I treat ALL unauthorized access as hostile and unfortunately there is no difference to your phone whether you, a police officer, a bad guy, a spy, or a thief is holding it. It only cares that you know the magic word to open it.

So play nice with your local authorities, and know that these strategies will most likely make them treat you as a hostile person.

# A SECOND, LESS IMPORTANT DISCLAIMER:

If you run these tools on your phone, you will most likely lose everything. I've tried out everything here except for order66, and I've lost all my data. Make sure to:

1. Back up all valuable data before trying any of this.
2. Try out each of these apps under different conditions to understand exactly when they fire.
3. Don't try this at all. Seriously, this setup is painful.

# Graphene OS

I've mentioned Graphene OS in my TAISA article, but I'm going to mention it here as well. The reason I'm including this specific Android ROM in these projects is because it's designed to treat the outside world as hostile and has security features that most other ROMs just don't have. Is it perfect for everyone's use case? Not if you're someone who prefers convenience (ala Apple), or someone who does everything through Google. If you're someone who values privacy over everything else though, I do think it gives you the best bang for your buck.

If you open your GrapheneOS setting app and head into Security & Privacy >> Exploit Protection, you'll find the following features.

## Auto Reboot:
There was an article recently that caught my attention. Police departments around the country were startled to find out that their suspects' phones were turning off by themselves. This was problematic because when a phone is unlocked, it enters a state called "After First Unlock", or AFU. In this state, a specially made forensic device can use techniques to siphon data off the phone. The following comes from the company Cellebrite, a big player in the mobile forensics game:

```
When in AFU iPhone state, the device has been unlocked at least once 
after it was powered on.  In this state, tools are able to collect a 
lot of information from the device.  If the phone is turned off or 
loses power, it will revert to the less helpful Before First Unlock 
mode.  Without the user password, BFU iPhone data collection is all 
you’re going to get.

-Cellebrite [https://cellebrite.com/en/what-can-be-recovered-from-bfu-data-collection/]
```
Supposedly this "Before First Unlock", or BFU mode has a lot less valuable data. 

At this time, people aren't quite sure what rules apple is implementing to judge whether the phone should turn off, but it seems to happen when it doesn't detect any entry to the phone for some period of time. Some reports said 4 days, others said 24 hours. 

Graphene on the other hand lets you choose. The Auto Reboot feature in the Exploit protection section will allow you to set a time between 72 hours and 10 minutes. If you don't unlock your phone within that time period, the phone will reboot itself. Why 10 minutes? Don't know. Maybe it's for people at protests? Or because they are super spies like Ethan Hunt? Either way, this feature is so frustrating to people with mobile hacking rigs that this single change to apple devices caused mass panic to law enforcement due to the lost data.

## USB-C port

Another fancy tool in Graphene's toolkit is the ability to turn off the data lanes in the USB port. Essentially, if a bad guy hooks up a malicious hacking device to your phone, or you decide to charge the device in a sketchy location, then the attacks will be useless because the phone can't pick up anything but power. Now I've been picking on cellebrite here, but UFED extractors aren't the only threat attacking your usb port. The following device is available for like $200 and it can do all sorts of malicious tomfoolery [https://shop.hak5.org/products/omg-cable].

This security feature gives you 5 options.

1. Off: Turns off the USB-C port. This option is so intense that you can only charge the phone when it's off. 
2. Charging-only: Turns off all non-charging USB functions (including headphones)
3. Charging-only when locked: If the device is locked, USB data is off. If the device is unlocked the port works as normal. It does note that if you lock a device with something plugged in, it will wait to turn off the port.
4. Charging-only when locked, except before first unlock: This one is worded a bit confusingly, but it adds an exception for the BFU mode. Now whether that exception turns on the data lines or turns them off, I don't know.
5. On: No restrictions on the USB port.

## Duress password

Now this next feature is not in Exploit protection, but is instead in the Settings >> Security & Privacy >> Device Unlock section.

Now we're cooking with gas. The secret agent has been captured, they're told to put in a pin to unlock the phone or they'll break another finger. The agent sighs and tells them the passcode. The bad guys type it in and suddenly, the phone turns off. They're puzzled for a second. Then a big circle appears on the screen with the word "Erasing underneath".

Yup, the bad guys have fallen victim to the Duress password, which is a code that will tell the device to erase all the data on the device. When you tell Graphene OS that you want to use a Duress password, it gives you a two-fer. You will get both a password AND a pin that will erase the phone. Don't worry, you set them yourself. 

If I were you, I'd put in my birthday. It's the first thing a snooping significant other or a spy is going to guess. Or maybe you should use something from the rockyou.txt list that hackers use when bruteforcing passwords. Or if you're afraid the bad guy is going to break into your house and steal your tech, why not use the router password? They're going to be pilfering it for data anyway. 

# Lucky's apps

"Well that's all well and good", you might be thinking, "but it doesn't help if I don't have the cash for a pixel." 

My bad, you're right. Why don't I give you some options for one of those cheap phones you buy at a grocery store or a mall kiosk. The first thing you'll need to do is download an app called F-droid. There are videos on Youtube that show you how to do this, but essentially, you go to the f-droid website, download the .apk file, and your phone will put up a security warning. You need to tell the phone to allow downloading third-party apps, then install it.

F-Droid is an app store for apps that are "open-source". Or in other words, it hosts a bunch of free apps where you can read their code if you're that kind of a nerd. Now, there is a developer on the F-Droid store that makes 3 really cool apps, and I don't see anyone talking about them. Their name is Lucky.

## Duress

If you were jealous that GrapheneOS gave you the ability to wipe your phone with a self-destruct duress pin, you're in luck. The Duress app has got you covered. There are 3 sections of this app. The second section allows you to set a duress password that'll wipe your phone. The third section will let you test whether the app is picking up your duress password without self-destructing.

The first section gives you way more more power though. Imagine for a second that you don't want to wipe your phone, but you do want to delete some apps. The first section lets you send a broadcast message to all the apps in your phone (within that user account). There are apps like Ripple that can be configured to "panic" when they receive certain messages and do things like hide or delete apps. This is a good way to trigger it.

Something to note about the duress apps, and any of Lucky's other apps. If you install them on guest user profiles, it won't delete them, it will just kick you out of the guest profile and put you back on the main on. I don't know why, but I'm guessing it's a permission thing.

## Sentry

Let's say that you don't want to neccessarily delete the phone if someone puts in a specific bad password. What if you want it to be deleted if someone puts in 10 wrong passwords? Sentry's got you covered. This app monitors your lock screen for failed attempts. You can set it to self destruct if someone puts in the wrong password X amount of times. 

What's more? It also has the ability (in some phones) to lock the USB controller so that an OMG cable can't impact it.

Even more? It's got a notification system. If someone tries to guess your password while you're in the bathroom or any app randomly gets internet permissions after an update, you're going to get a heads up so that you can be on your toes.

Lastly, it supposedly gives you the ability to prevent your phone from being booted in Safe mode. I've never gotten that feature to work though, so mileage may vary.

## Wasted
Ladies and Gentlemen, I saved the best for last. If you want your phone to delete itself, you've come to the right place.

The main page is simple. You have 2 checkboxes and a button. The first checkbox tells you it'll wipe the whole phone. The second checkbox is for your eSIM, for if you just want the ability to call from your phone number to be erased (not useful if you use a physical sim card). The button is for putting the app in Administrative mode. The app will register itself as your phone's boss so that it can delete whatever you tell it to.

Next, there is a gear icon in the top right corner. Click it. The following are all the ways that you can tell your phone to explode.

1. PanicKit: If you use an app like ripple, you can tell it to tell Wasted to wipe your phone.
2. Tile: This one puts a malicious airplane mode button in your quick setting toggles from the top of your phone. If a bad guy tries to put your phone in airplane mode, it will self destruct.
3. Shortcut: There will be a button on your app screen. You press it, kaboom.
4. Broadcast: You have tasker on your phone? Tasker is like a swiss army knife of phone automation. It can send texts on your behalf, send ssh commands to servers, play a cheeky song on the phone, then trigger the self destruct through this Broadcast mechanism. You can literally program tasker to listen to the voice command, "execute order 66" and the phone will log into all of your infra, nuke them, and play the imperial death march before wiping the phone.
5. Notification: The app will read the notifications and if it sees a specific message, it will erase the phone's data. Imagine texting the phone with telegram and it dies.
6. Lock: You can set a timer to go off every time someone presses the lock button. If someone doesn't unlock the phone, it won't just reboot, it will erase. And you thought Auto Reboot was alarming! Side note: If your phone reboots or shuts off while unlocked, it won't tell the timer to start.
7. USB: Ok, this one is actually my favorite. You can tell your phone to self-destruct if it detects a data connection. So if a bad guy manages to bypass the Graphene USB protections, it'll hit this and erase it. It makes it so that you literally can't use USB attacks.
8. Application: Ok, so this one is devious. Most people who want into your phone want your text messages. This one installs fake text applications for signal, threema, and telegram on your phone. If someone tries to read your texts, KABOOM.

# Summary

So imagine a phone that will erase itself if:
1. You guess the password wrong 10 times
2. If you guess a specific common password
3. If you don't unlock the thing for X amount of time.
4. won't let you hook a usb device to guess passwords
5. you managed to hook up a usb device that hacks the usb port open.
6. someone tries to read your texts.
7. someone tries to put your phone in airplane mode.
8. if some weird custom automation from tasker goes off.

AND THEN, EVEN IF YOU DON'T GET IT TO SELF DESTRUCT:
1. It will Auto-reboot in 24 hours.

And we haven't even talked about guest accounts, the new private space feature, or work profiles which all have seperate encryption keys locked behing different passwords.

I won't lie, I ran this setup for like 5 months before and I wiped my phone like 6 times. It takes a REMARKABLE amount of discipline not to trigger one of these traps. I do not recommend ANYONE runs this setup unless... I don't know... you're Edward Snowden? Idk, I bet he doesn't even have a smartphone and opts instead to use his laptop for all communications. 
