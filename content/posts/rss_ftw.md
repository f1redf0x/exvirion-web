+++
title = 'RSS is the best of the Internet'
date = 2025-04-17T11:30:41-05:00
draft = false
tags = ["Housekeeping", "digital minimalism"]
categories = ["Digital Minimalism", "Text is King", "Gems"]
+++

So I've mentioned quite a few times that I use [a program called newsboat](https://newsboat.org/) as my primary way to access the internet. I use it to:

* Subscribe to all the blogs I love to read

* Subscribe to all the Subreddits that I like to see

* Subscribe to all the podcasts that I like to listen to (although Castero is good for this too)

* Subscribe to all the Youtube channels I love to view

* Subscribe to Vulners rss feeds for companies that are releasing CVEs and exploits.

* Subscribe to Job boards for companies I'd like to work with one day (although that list is small and shrinking as companies remove their RSS feeds)

On a given day, I might only use the search function of my browser a time or two to get answers that I need, and even then, I find my local offline copy of Ollama has a lot of "good enough" answers to get me what I need to know.

That said, for those who aren't familiar with what I'm talking about might need a bit of an intro to RSS to fully appreciate how much nicer it is to fetch your updates instead of being "terminally online".

Side note: When I was a bit younger, everyone who had ever played the Legend of Zelda complained that they hated characters like Navi and Fi because they would stop your journey to tell you to listen because she had a notification for the player. I don't understand how notifications don't get the same level of hatred or scrutiny. I imagine it's a "frog boiling in water" kind of thing where you need notifications for phone calls, so you must need it for other apps? IDK.

# RSS: Protocol of Champions

In the early day of the web, there was a problem. Websites didn't have this two-way relationship with computers that they do now. Companies and Bloggers didn't have apps with full notification stacks, they had websites. 

Websites are really meant to be a one-way technology, and until apps and websockets became a thing, kind of had to be. People's computers are behind firewalls and firewalls only allow traffic to computers if the user initiated contact. 

Side Note for the neckbeards: Yes, I know this is technically inaccurate because the protocol wouldn't even work if the server was initiating the connection because what would the browser even do with that info?

Website servers don't know that your computer exists until you reach out first by clicking on the website link. But once they know your IP address, they can't just reach out to you whenever they feel like it. They need to wait for you to initiate a new connection. This is partially because of firewalls, and partially because it saves bandwidth. It's a good system; I'm a fan.

This meant that users had ultimate control over their connections to servers, but it also meant that they had to manually go to every website that they liked to see if they had an update. It's like when you post a comment and furiously refresh the page to see if the likes go up. It's a lot of wasted clicking.

The first solution to this issue is still going strong today and that was mailing lists. A company or blog would ask for your email which got added to a script that sent you an email every time the website got updated.

This was solid, and still is, but you've seen peoples' email inboxes. FLOODED. Also, tweaking email frequency is a bit of a challenge. Too many emails and your users might remove themselves from your email list. Too few, and interest in your site drops. There is a goldilocks zone with emails.

This is where RSS and it's younger cousin ATOM come in.

RSS or "Really Simple Syndication" is an XML page that constantly gets updated whenever something new happens on a website. If you go there manually, you can see the titles of articles, all the article text, etc. 

My website has RSS enabled. If you go to the archives tab and look for an icon that almost looks like a "wifi" logo on it's side, that's my RSS feed. There is also a unique RSS feed for each category and tag on my site. 

Assume that you don't care about anything on my blog except for the Android stuff. You can grab the RSS feed for [just the android stuff.](https://firedfox.netlify.app/tags/android/index.xml) This is true for every tag. It allows you to get exactly what you want, and nothing more. 

A lot of news sites like [CNN](http://rss.cnn.com/rss/cnn_topstories.rss) and [FOX](https://moxie.foxnews.com/google-publisher/latest.xml) have their own RSS feeds. 

The best part? You don't have to check these feeds unless you want to. No more notifications. When you get bored, you just open your RSS reader app, all your feeds get refreshed and you can look at the stuff you care about.

# RSS Readers

There are a ton of RSS readers that you can choose. I can't tell you which one is the best. There are GUI ones like Feedly, Newsflash, or even email clients like Mozilla Thunderbird (though, I'd stay away from Mozilla, tbh). There are terminal ones like newsboat, nom, or neix. 


I personally use newsboat because it works well with my operating system. You can configure it to launch specific RSS links in other applications. All my youtube stuff gets launched with mpv. All my blog stuff is opened in lynx. All the comics I like are opened in Brave or examined for pics and pulled into sxiv. I might write an article about how I set up my config file, but the point is there is no shortage of options, so experiment and go nuts! Find one that works for you!

# A quick note on ATOM vs RSS

There are technical differences in the spec, but for the most part they are exactly the same to most readers. Grab the link and your reader will tell you if it can be parsed for content.

# How to find RSS links?

Look, I could tell you that to find Youtube rss feeds, you should go to a Youtube channel's videos page, look at the page's source code, and look for the RSS feed there.

I could also tell you that you can take most blogs and slap /feed to the end of the url, or .rss, or .atom and you'll get an RSS feed.

I can even tell you to look for the button/logo.

But honestly?

Go to [this website](https://rssfeedasap.com/rss-feed-from-youtube). It has a collection of websites at the bottom of the page. Find one you like and start pasting a user's profile into the box and 9 out of 10 times, you'll get the link.

# Take back your attention

I legitimately think that smartphone apps were a mistake. If I could, I'd roll technology back to the point when Steve Jobs introduced the Iphone and force progressive web apps to be the way you install software instead of these intrusive bi-directional demons that live on your phone, spy on your activity, and bother you daily with the "HEY, LISTEN, WATCHOUT" notifications that drive people to scroll constantly.

If you want to reduce your dependence on the internet, set up RSS feeds for the stuff you really care about and delete the apps for those services. You dictate when you want to be bothered by a website; don't let it be the other way around.

Or not. Do what you want. You're going to anyway. XD
