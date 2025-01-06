+++
title = 'How to Make friends with Bash!'
date = 2025-01-05T19:13:14-07:00
draft = false
tags = ["scripting", "lynx"]
categories = ["Social"]
+++

This'll be a short one, but I recently moved to a new city and it's always difficult to meet new people if you aren't part of some large organization or religion. One way to get familiar with new people is to use `https://meetup.com`. The issue I was facing is that the city I'm a part of is huge and there are a ton of groups. Trying to login to each group individually would take like 20 minutes if I just wanted to see what they were up to. So here's a little script that I made with chatGPT and some unix philosophy magic that will give us all of the group urls, event names, and times so that I can see if any of them interest me this week.

Here's the script:

```
#!/bin/bash

# Check if a file is provided as an argument
if [ $# -ne 1 ]; then
    echo "Usage: $0 <file_with_urls>"
    exit 1
fi

# Check if the provided file exists
if [ ! -f "$1" ]; then
    echo "File $1 not found!"
    exit 1
fi

# Clear results.txt or create it if it doesn't exist
> results.txt

# Read each URL from the provided file and run lynx dump
while IFS= read -r url; do
    echo "Dumping: $url"
    echo "$url" >> results.txt
    lynx --dump "$url" | awk '/Upcoming events/{flag=1} flag; /Past events/{flag=0}' | grep "\*\ \["  >> results.txt
done < "$1"

```

You invoke the script like this:

```
./meetup_scrape.sh urls.txt
```

The script is pretty self explanatory with the comments, but the gist is:

1. It sets the interpreter to bash
2. It checks to see if you provided a file of line seperated URLs as an argument to the script.
3. It check to make sure you didn't mistype the filename.
4. It wipes the previous results.txt file (if you've ran the script before)
5. It opens the url file in a do while loop.
6. It writes the url to the results text file.
7. Lynx dumps the content of each url and passes it to awk.
8. Awk saves only the text between "Upcoming events" and "Past events", then passes it to grep.
9. Grep only grabs the line with the lynx boxes, which denotes title and time and sends it to the results file.
10. It ends once it's gone through the whole list.

The result of this script is that I have a list of all the activity names and times.

If you couldn't tell by the fact that I have  a friend gathering script in bash, I'm an introvert, so most of these meetings won't make the cut. But if I see one I like, the url for that activity is right there for pasting so that I can get the address.

I don't see the need to take those items and put them in my calendar, but I think that would be an improvement that could be made to the script.
