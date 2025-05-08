+++
title = 'Enumerating Subdomains with crt.sh'
date = 2025-05-07T20:05:50-05:00
draft = false
tags = ["scripting"]
categories = ["QTT"]
+++

There are a lot of tools you can use to see what a company is up to techwise. Dig is good, whois is good, spidering through webapps looking for links and apis is good, but my favorite... really dead simple way of getting information on a domain is to just checkout https://crt.sh.

# The script

To even call this a script is an insult to scripting. I'm just curling the csv endpoint of the crt.sh website and using some cli-foo to turn it into subdomains. 

```
#!/bin/bash

# Prompt the user for input
read -p "Please enter the company you want the subdomains of: " X

# Run the command with the user input
curl "https://crt.sh/csv?q=${X}" | cut -d, -f5 | sort -u | grep \"
```

Put this in your ~/.local/bin folder as "subdomain".

To use it, type `subdomain`. It'll ask you for a domain. Give it one like google.com. Badabing, badaboom, look at all those pretty subdomains, ready for a probing... (Provided you got permission. You did get permission right?!)
