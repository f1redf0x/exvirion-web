+++
title = 'Your security is backwards, Sir!'
date = 2025-04-15T12:03:57-05:00
draft = false
tags = ["Vulnerability-Management", "CyberSecurity", "HouseKeeping"]
categories = ["CyberSecurity", "HouseKeeping"]
+++

Did you know that on average a CISO, the executive in an organization that focuses on cybersecurity, [has a shelf life of 18 - 26 months](https://www.ciso.inc/wp-content/uploads/2023/08/CISO-Report-2023-.pdf)? This number has been estimated to be as low as 17 months [by other groups](https://www.tenable.com/blog/the-average-ciso-tenure-is-17-months-don-t-be-a-statistic). 

Why do you suppose that is?

Easy, because no general can win a war on multiple fronts. I have no numbers to back these claims up, only anecodatal experience:

- Vulnerabilities are discovered and found on systems faster than Remediation Teams can fix them.

- Services and machines are being spun up faster than the Asset Management team can track.

- Creating Ransomware [takes about 30 minutes, less if you know what you are doing](https://www.youtube.com/watch?v=UtMMjXOlRQc).

- The laws in place to protect computers are cumbersome, tedious, and don't focus on the ways attackers actually compromise companies.

- Development practices are required to be so fast, cheating and accruing technical debt is mandatory and exponential.

- Some vulnerabilities don't have a simple, trackable mechanism like a CVE despite being dangerous. (default passwords, anonymous ftp priv-esc, etc.)

In short, there is too much to focus on and most of it doesn't actually meaningfully help you keep safe.

# So what the heck happened?

This overwhelming deluge of work to do is the result of an industry that was build iteratively and not architecturally. 

Imagine a world where wifi doesn't exist. It makes sense that the biggest threat comes from outside the network. So network architects build a moat to keep the bad guys out and that works for a while.

Now imagine that the world suddenly got wifi. Now, defenders are fighting a war on external traffic, and devices inside their networks.

Now imagine a bunch of laws were made thinking of a specific business or entity. If a business is small, 7 days may be enough time to remediate a vulnerability. If a business is large, there might be so many that it might take a month. It makes sense that if law makers were thinking of a hospital or a small business that the controls that work there might not scale up or down.

Now imagine that you are struggling to keep up with the fight against external foes and malicious wifi users, then you find out that governments have their militaries try to break into your company. Could you imagine the paranoia? 

Now imagine that you are told that this fight, that you were already losing is about to ramp up to 11 because all these bad guys now have AI.

What do you think would happen in such a world? I bet you'd work the job for 17 months and quit.

# Defense in Depth

Notice that in the previous section that the protocols being put in place started outward and moved inward.

- start with the firewall

- seperate the business network from the complementary wifi

- set up checks for phishing

- focus on physical security

- and on and on

This methodology is called "defense in depth" and it's a good one. Put meaningful layers of protection between the things you care about and outside threats. This is a strategy that works for computers just as well as it works with castles.

# The Inversion

Here's my hypothesis. I think that Cybersecurity teams get lost and confused when they try to setup all of the protective layers at once or they try to build their security from the outside to the inside, which is the opposite of good practice. 

- Do they focus on vulnerability count?

- Do they focus on criticality?

- Do they focus on maintaining a firewall?

- Do they focus on worker's machines?

- Do they focus on servers?

- Do they focus on legal compliance?

Jeez, that's a lot; but it's also not prioritized. Let's take a step back and ask the big question:

```
What are we doing all this work for?
```

Answer? Keep the business running and customer data safe.

# An easy place to start: Step 1, save the King, Queen, and Treasury (Critical Focus)

Ok, so forget literally anything anyone tells you and start here:

1. What services, data, and applications cannot go down? Write them down.

2. What data do customers have that's legally protected? Write that down.

Awesome. Now completely disregard everything else for a minute. Assume that a disgruntled employee at the company wants to take the company down. This is your most powerful threat. What could this theoretical terror do to your stuff given that he's already on the network and authenticated? Great. Now work on preventing that guy from taking down items 1 and 2.

If you are lost on where to start, I recommend securing Active Directory, DevOps, Your Databases, Your Networks, Your CMDB, and then your applications (In that order). Active Directory often gives you access to every windows device in the domain, including windows devices with ssh keys that you can use to login to DevOps stuff. Your DevOps infra contains all of your keys, secrets, and access to your databases. Your Databases need to be secure from inside threats first (authentication and authorization), then external threats second (network). Your CMDB will allow you to rally a defense when you are being attacked. If you don't know who is in charge of what, you'll be mincemeat to a decent attacker. Make sure your applications are hardened, monitored, and updated. If you do just these 6 things, you will be better off that most fortune 500 companies.

A special note on PCI DSS: I consider PCI DSS to be a part of Applications. Protecting it must be high on the list, but if you don't protect the supporting infrastructure first, you're never going to be compliant anyway.

I call that process "Saving the King, Queen, and Treasury". If the archers, moat, castle walls, and inner courtyard fail, you should still be able to protect the King (Crown Jewel Services) and Queen (critical data) from a spy, assassin, disgruntled knight, or invading force. If you can protect the treasure (financial and ERP systems), than no matter how badly the kingdom hurts, the King/Queen will be able to mount a defense.

# Step 2, Save the Nobles (High Focus)

Now that you've protected the stuff that will kill your business if you lose, you need to secure the people closest to those assets. Executives, HR, Accounting, IT, and Sanitation need to be scrutinized and protected. All 5 of those departments have direct or near direct access to all the business critical functions. You need to make sure:

1. Can you track when and how these people access the treasure?

2. Are their methods of access safe?

3. Do they have enough power to do their jobs, but no more?

4. Can you take away their power if they become threats?

5. Are they trained to spot deceivers?

If you can confidently state your position on these 5 questions, you will be better off than most.

Now you might be surprised that I said Sanitation. Janitors have physical access to every square inch of the castle. The scrutiny on their actions must be top of the line because a malicious janitor can do unfathomable damage.

If you have heard of "Least Privilege" or "Zero-Trust" this is where you'll implement those. Make sure that every key player is only doing their assigned role and nothing else... at least in regards to interacting with the King, Queen, and treasure.

# Step 3, Save the Peasants (Medium Focus)

Now that you've managed to protect your key resources, and you've both vetted and protected your privileged classes that have access to sensitive data, now you need to protect the assets of the workers. If the workers get sick, they can spread their illnesses to the upper class and potentially to the King and Queen. 

This is where you typically focus on antivirus, laptop monitoring solutions, Access control for things like VPNs, badges, etc.

If you are constantly struggling to meet cybersecurity deadlines as an executive or a security manager, this is where I would recommend spending less time. The peasants make up a vast majority of devices, but they have the least access to things that can cripple you.

# Step 4, Construct a Castle Wall (Critical in the beginning, Low Focus throughout)

Now that you've protected all of the people INSIDE the company, it's now time to repel the invaders. Now, when you start out, you'll likely have a firewall because IT pros have learned that blocking all communications that are started from the outside is a smart move. You'll want to make sure that check is there when you very first start and that it is maximally restrictive to outsiders.

Once you've gone through the efforts described above, it's now time to tune those walls according to the threats you've been encountering. Maybe people have been exfiltrating data. Work on those Egress rules. Maybe there are certain services you want to let into the network. Make an exception to let that trusted traffic in. Maybe you want to set up both a network perimeter and host based protections. So even if a bad guy gets in, each peasant cottage is its own fortress.

If you've worked carefully on the previous stages, then nothing should be able to get through, even without a firewall in place. Also, these tend to be rock solid once they're setup, so focusing a lot of time on them doesn't make sense. When you're dealing with fires, focus on the most important stuff first.

# Your Security Is Backwards, Sir!

I hope that this metaphor helped to put security in perspective. Protecting assets is a Sisyphian task that you'll never get 100 percent right. So stop trying. Focus on protecting the most important stuff, and once those are reasonably secure, work on protecting that things that connect to them, then the things that can infect those things.

Don't spend your time working on the wall when a rogue janitor can kill the King. Do everything in order, and everything will fall into place.
