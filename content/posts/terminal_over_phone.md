+++
title = 'Terminal Over Phone'
date = 2025-04-25T17:51:51-05:00
draft = false
tags = ["Android", "digital Minimalism"]
categories = ["Digital Minimalism", "Privacy"]
+++

If I could pick a theme for this blog, it would most likely be "digital independence". I may spend some time focusing my efforts on hacking, productivity software, command-line ballyhoo, or general philosophy, but in general I focus my efforts on tools and techniques that make me less reliant on others.

- Why use Gitea if you can use ssh and git?

- Why use Google Authenticator if you can use GNU pass?

- Why use OBS-Streamlabs if you can use ffmpeg?

It's not just that I value open source, which I do. It's that these tools will always be around if you are willing to put in the effort. Look at all the products Google and Microsoft have thrown away. [Especially Google](https://killedbygoogle.com/) And Linux? If you want, you can download every Linux package on a mirror and keep it for a future you [as seen here](https://computingforgeeks.com/creating-ubuntu-mirrors-using-apt-mirror/).

# The Cell Phone

Not to come across as some paranoid nutjob (which I am), but the cell phone is as dangerous as it is useful. It:

- Tracks you whether you want it to, or not. [Google](https://apnews.com/article/828aefab64d4411bac257a07c1af0ecb) and [Apple](https://gizmodo.com/apple-iphone-analytics-tracking-even-when-off-app-store-1849757558) are both guilty of over-riding your choices.

- Is designed to be [highly addictive](https://www.npr.org/sections/alltechconsidered/2017/03/13/519977607/irresistible-by-design-its-no-accident-you-cant-stop-looking-at-the-screen).

- And it's part of a large scale plan to make you [pay for everything more than once](https://www.theverge.com/2022/7/13/23206999/car-subscription-nightmare-heated-seats-remote-start). After all, if everything is an app, then any company can make it [illegal for you to fight back against abuse](https://www.eff.org/deeplinks/2019/06/felony-contempt-business-model-lexmarks-anti-competitive-legacy).

So it's creepy, addictive, and actively plotting against your wallet. Not to mention that it'll [use your speakers to emit a high-pitched sound to alert devices of your presence.](https://www.wired.com/2016/11/block-ultrasonic-signals-didnt-know-tracking/).

In short, it's a device that was never meant for you. It was a device that was meant for corporate interests. To quote Iraq from the watch_dogs game ["Who do you think is going to win between you... and business?"](https://youtu.be/02IK1LNF2o8?si=rQZqpWxKTR9Far7f&t=44)

A couple years ago, I was tethered to my smart phone and I hated it because of how dependent I had become on it, but I didn't have the skills to say no to it. I needed a phone number, an OTP app, encrypted messaging, my contacts list, a youtube client, etc.

This year, I'm close to being free from a smartphone running my digital life and I'll share some of the replacements I use.

# A Phone number:

So this one is tricky. Anyone who says "just use Google Voice" clearly doesn't use ONLY google voice. A lot of institutions require a SIM card fixed phone number to do things like banking, credit, online services, etc. There are 3 tricks that I can recommend.

1. Buy a sim card, move every account you want associated to that number to it, then after 30 days, port the number to a voip service. Most institutions don't check to see if your number is VOIP after you register it the first time.

2. Cheogram. There is this phenomenal service called jmp.chat that lets you use a voip gateway with XMPP. It's fairly technical, but after youregister a phone number on it, you can call anyone's phone number, using any number of compatible XMPP applications. You can even [pay with crypto if you don't want the bank to know](https://kb.above.im/jmp-chat/). I use Dino on my desktop, but there are plenty others. Some institutions aren't familiar with cheogram and you may be able to bypass them.

3. Call and text forwarding. I'm going to be experimenting with this soon, but I suspect that you can configure a cell phone plan to ring another number if the phone is unresponsive (because you took the battery out). I don't know how reliable this technique is, but I think this represents the best of both worlds as you don't have to carry a phone, but you can still have one in cases of emergencies.

# Password Management and OTP codes:

I used to use 2 separate services for these two functions. I used Authy for OTP codes and Bitwarden for Password management. I decided to abandon Authy after 3 years of service because they decided to just kill their desktop app which I relied on. So I purchased the Bitwarden paid service which gave me OTP codes.

But then I started to get worried.

I work with a lot of FOSS products and the most popular monetization strategy they have is to charge businesses, but leave customers free. But within the space of a year, I had lost like 4 or 5 open source products due to licensing changes and I had this idea that if Bitwarden wanted to, they could jack up the rates... or remove the OTP feature... or something else. I asked them what assurances they could give me that they would respect customers in the event of a marketing change and they essentially told me to take it or leave it. 

That's fine; they have the right to change their product whenever and however they want. But then, about a month later, I had three products in a row refuse to let me in despite having the right password because it's up to my phone and the company's app to decide whether I was worthy of logging into their service. (This is also why I don't use a phone if I can help it. If you log in ONE TIME, your account will make your phone the one source of truth. And if you deleted the app... You're done.)

I was fed up, and I decided that if I can help it, I will always use the more cumbersome non-app process for any service. If I can't use your service with a web browser or in person, you've lost my business.

So at this point, I moved to KeepassXC which is a fantastic open source project that does both passwords and OTP codes.

But again, there was an issue. [One of the Debian maintainers arbitrarily decided to remove the browser integration from KeepassXC](https://www.theregister.com/2024/05/22/apt_gains_keepassxc_loses/). When I talk about digital independence, things like this are exactly what I'm trying to innoculate you from.

So I moved from keepass to pass and I have an offline copy of the source code for pass and all of its extentions. 

# Encrypted Communications:

One more thing that was crucial to me was to have encrypted text messages and calling. Ever since the Snowden leaks showed the extent of the NSA's dragnet surveillance program, I've been a heavy supporter and advocate for the use of encrypted chat apps. And I've been [proven right by the FBI](https://www.npr.org/2024/12/17/nx-s1-5223490/text-messaging-security-fbi-chinese-hackers-security-encryption) who admitted that it wasn't just the Americans spying on everyone's phone calls and texts.

Now in order to satiate my family, I used an IPhone for a couple years because although it's not the best solution on the market, IMessage and Facetime seem pretty good from what I've read of the tech on various papers, DefCon talks, and Apple's privacy policy. But Apple made a couple of mistakes that strike me as very "Pro Big Brother".

1. [They tried to implement a service that scanned all of your photos for illegal material](https://www.wired.com/story/apple-csam-detection-icloud-photos-encryption-privacy/). This might be ok in theory, but as Google showed us [that this technology has false positives and it can ruin your life](https://www.newser.com/story/324535/dads-took-medical-pictures-of-their-sons-lost-their-google-accounts-forever.html). 

Also, from a guy who's been watching this for ages, whenever the government wants to take away your privacy, they'll usually rely on drug-dealers, money-launderers, terrorists, and pedophiles to make their case, which is so common, it's [got a cute little nickname](https://en.wikipedia.org/wiki/Four_Horsemen_of_the_Infocalypse). Also rest assured, that although the invasion starts by stopping the big crimes, it'll always trickle down to [others](https://www.vice.com/en/article/period-tracking-apps-privacy-data/). EDIT FROM THE FUTURE: The above article is not [theoretical anymore.](https://www.eff.org/deeplinks/2025/05/she-got-abortion-so-texas-cop-used-83000-cameras-track-her-down)

2. [They deleted an app that protestors were using to coordinate and keep safe](https://www.vox.com/recode/2019/10/23/20927577/apple-hong-kong-protest-app-democracy).

So because of this happening back to back, I decided to move back to android and use [Signal](https://signal.org/download/). It took a while to get the family on board, but they came around. 

Now this presented a challenge because Signal is smartphone only. There is a Desktop app, but it must be linked to an existing instance of Signal. 

Actually, this is a big enough obstacle that I'm considering moving off of signal and just using XMPP with OMEMO and giving my friends and family user accounts.

Until that though, I'm going to rely on the [signal-cli](https://github.com/AsamK/signal-cli) tool. It allows you to register your command line to Signal's servers. It also uses some slick command line tools to read the QR code that the desktop produces to link it.

# Contacts List

Just use [abook](https://github.com/hhirsch/abook). I exported my contacts list to a csv file and used abook's native conversion tool to turn it into an abook file. It also integrates swimmingly with mutt.

# A Youtube Client

So I very rarely use Youtube directly. I typically use RSS to subscribe to all my channels and use [mpv](https://www.linuxfordevices.com/tutorials/linux/watch-youtube-videos-on-mpv-player) to watch the links from the rss feed.

# Summary

So in short, my laptop handles my phone calls, my passwords, my otp codes, my friend's list, and my access to Youtube. Those were all the functions that I relied on my phone for. The rest can be handled via my browser or some command line tools. My phone sits there idle on my desk, ever losing charge, never being turned on. Hopefully, one day soon, I can get rid of it forever.

Just the way I like it.
