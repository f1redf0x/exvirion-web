+++
title = 'Gopher'
date = 2025-04-13T08:58:04-05:00
draft = false
tags = ["Indieweb", "plaintext", "lynx", "digital minimalism"]
categories = ["Text is King", "Gems"]
+++
# The Old web was better

I hear a persistent complaint levied by people who have been using computers since Dial-up was a thing.

(psssst... kids... Dial-up was an old technology where you gave your computer a phone and it would scream at another computer over the phone in some weird machine language. If you picked up the phone, you could hear them talking.)

You can see this argument on:

* [Reddit](https://www.reddit.com/r/Older_Millennials/comments/1dle7h5/who_remembers_the_old_internet_was_it_better/?rdt=62684)

* [The Atlantic](https://www.theatlantic.com/technology/archive/2024/03/old-internet-web-emulator/677845/)

* [SffWold](https://www.sffworld.com/forum/threads/i-miss-the-old-internet.57195/)

* [Engadget](https://www.engadget.com/2013-01-23-the-internet-used-to-be-better.html)

* [The whole IndieWeb Community](https://indieweb.org/)

* Etc

There are a number of common arguments you hear about this subject.

- The old internet required netizens to have a certain level of tech skill / IQ in order to create content. This means you had to be literate enough to write html.

- The old internet had an expection of psuedonymity. A lot of people say "anonymity", but it was common for people to create a reputation attached to a handle. If you got too uppity with a handle, it was burned and you'd need to create a new one and your rep started at zero.

- The old internet didn't require a phone number, multi-factor codes, terms of conditions, etc.

- The old internet was text based. It was really easy to parse and make tools for.

- The new internet is slow and bloated. Sometimes it takes 20-30 seconds for things to load.

- The new internet is full of ads, dark patterns, and manipulative technologies.

- The new internet is dynamic. It constantly changes, making it hard to write tools for.

- The new internet is corporate. If you want to post, you need to follow the rules of social media entities that have a vested interest in making their website "advertiser friendly".

These distinctions lead to the creation of new terms to discribe the web. Web 1.0 is the old internet and Web 2.0 is the new internet.[Wikipedia](https://en.wikipedia.org/wiki/Web_2.0#Web_1.0). Some people say that Web 2.0 started with smartphones. The logic there is that the smaller screens and the phone's processing power required new, more dynamic web content. Some say that Web 2.0 started with javascript, since javascript allowed for dynamic webpages. Either way, the difference seems to be static vs dynamic content.

If you've got static content, you don't NEED to track users because every user gets the same data. You might choose to do light auth for the sake of funding the site and giving access to paying users, but there is no need for tracking everything the users look at. That's not to say that you can't do surveillance with a static website, but it's OPTIONAL by design. The content doesn't change, so the need to track users is relegated to one demographic, "the audience".

Meanwhile, if you have dynamic content, you MUST track users. You can't show a person of a particular political persuasion content that will make them leave the platform, therefore they must login. In order to curate that content, you MUST profile the user to find out what they like. In order to create that profile, you MUST surveil the user. This requires scripting, click tracking, analytics, etc. And if you are already surveilling your users, you might as well use this profile to advertise to them. In this paradigm, your demographics get more and more granular as you search to provide the perfect content.

# So What?

Ok, so let's say you agree with the hipsters and the boomers who think that the internet was better in Web 1.0. Is there any remnant of that Old Internet?

Turns out, yeah... absolutely there is. 

Imagine an internet that has no ads, no tracking, no authentication, no javascript, no pictures (sort of... you can still download pics and music), nothing. It's text and the nerds who know how to write just enough code to share their thoughts.

This is what [gopher](https://en.wikipedia.org/wiki/Gopher_(protocol)) is. Gopher is similar to html. You write text documents that display your thoughts, and you need a browser to interpret those text documents into a website. There are thousands of gopher sites that you can access and read. You can even join Gopher communities by finding combined hosting servers.

Now, one of the benefits of having a second internet that's harder to access is that you need a guide to know how to access it. Let me help you.

## Step 1: You need a Gopher Browser

There is a text based browser called lynx. This is a command line tool. Download lynx for whatever Operating System you have. You can get lynx from [https://lynx.invisible-island.net/](https://lynx.invisible-island.net/). 

It's also in every package repo ever. If you've got a Debian-based system like I do, just run `sudo apt install lynx -y`.

This tool is easy to use. Just type "lynx http://somewebsite.com" and this browser will open the text version of the site. 

Note: Lynx is a weird, but delightful browser. It can't render javascript or media. It can download any webpage you go to for permanent keeping. It also has a text dumping mode so that you can write tools to parse text. I recommend playing with it, because it makes writing tools REALLY easy.

## Step 2: Set up aliases

I only really use 2 gopher links because they give you the best starting point. Open your .bashrc or .zshrc and add these lines:

```
gopher='lynx "gopher://gopher.floodgap.com/1/v2"'
phlogs='lynx "gopher://sdf.org:70/1/phlogs"'
```

Then run the command `source ~/.zshrc` or `source ~/.bashrc` to tell your terminal to use the aliases.

## Step 3: Veronica

So now that you have the lynx browser set up, and the aliases configured, open your terminal and type `gopher`. Gopher is my alias for the gophersite "gopher://gopher.floodgap.com/1/v2". Think of this website as the gopher google's home page.

Now, on this page there is a link called "Search Veronica-2". Veronica is a search engine for gopher sites. Use lynx to navigate to that link and use the right mouse key to go to the Veronica-2 page. Then, type your search term and it will pull all the gopher sites that match your search term. Feel free to explore. It's as close to the old internet as you can get without needing an [isp hookup](https://en.wikipedia.org/wiki/Usenet).

## Step 4: Phlogs

After you've played with Veronica for a while, let's look for phlogs. In your terminal, type `phlogs`. This will open a community server that's still quite active. SDF.org is a gopher server with a few dozen phlog (gopher blog) sites. This is where I go when I want to check into some of my favorite phloggers. 

Here, you'll find diaries, debates, poetry, philosophy, and even music samples. A lot of phloggers also provide IRC, XMPP, or email ID's in case you want to talk to them. It's pretty nice. And since there isn't really a forum here, the drama is more... civilized? That's probably the right word. You'll see heated debates, but you need to go to each phlogger to see what their opinion is. There is no interruption, no hateful comment sections, nothing like that. It's just phloggers sharing opinions on their own phlogs about other people's phlog articles. It's really refreshing, to tell you the truth!

# Summary

So, if you agree with the idea that Web 2.0 is a mistake (or just exhausting) try giving gopher a go. If you like it, there are other protocols like Gemini that you can look up. If you want pictures and music, you might try [neocities](https://neocities.org/browse) on your normal browser instead. Finally, if you want websites that aren't very bloated, I highly recommend [the 1MB Club](https://1mb.club/).
