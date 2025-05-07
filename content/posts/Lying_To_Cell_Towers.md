+++
title = 'Lying To Cell Towers'
date = 2025-05-07T15:50:58-05:00
draft = false
tags = ["data"]
categories = ["Spy-phone"]
+++

Not sure who needs to know this, but here's some fun trivia for you:

[You can send text messages from 4G/LTE enabled routers.](https://docs.gl-inet.com/router/en/4/interface_guide/sms/). 

This functionally means that if you were particularly disciplined with how you dealt with texting, you could place one of those routers literally anywhere in the world, use a compatible SIM card that's attached to your identity, and you have yourself a little covert operative that misleads your ISP into thinking you are somewhere you are not. Make sure that all of the texts you make to banks, businesses, etc are run from that router and SIM.

After all, all the cell tower sees is that a device with your SIM card is in a tri-laterated position. It might not see the ethernet connection to a building with a different ISP that you use to connect remotely to the router to send the text messages.

# The Inspiration

I've been thinking of writing a short story or novel about a "digilante" that's trying to take down a cyber-sophisticated syndicate. 

In almost every vigilante versus mob story, there is a point where the bad guys think they have the drop on the hero and use some illegal access to an ISP to track the hero down and kill them. I was trying to figure out what kind of opsec tricks a person might use to mislead such a dangerous foe, and that seems like a good one to me. 

It's not the only one though. You can also:

- Use a Linux Pinephone with SXMO to [text using ssh](https://sxmo.org/).

- Use Android's adb bridge and a vpn to the phone [to send a networked text command.](https://accessibleandroid.com/how-to-make-calls-and-send-text-using-adb-commands/)

- Give an AI access to an android phone [with the same trick.](https://github.com/evilsocket/nerve/tree/main/examples/android-agent)

# Weakness

This is definitely the sort of thing that would work better in the realm of fiction than in a real practical way. Firstly, the IMEI of the router would expose the ruse immediately because as far as I'm aware gl-inet doesn't make phones. Secondly, I don't know of a method to use bilateral audio over SSH to place a call on the router. This means that a digilante type would need to text and never place a call. This could be seen as suspicious unless the hacker used ONLY-TEXTING as a kind of calling card or MO.

Still if it was played up in a game of cat and mouse where it looked like the leak of the phone number was a mistake, then an attacker might not think to check details like that in their hurry to exploit their target.

