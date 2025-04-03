+++
title = 'Align your spine: Minding the gaps in your xybersecurity program'
date = 2025-04-03T13:42:34-05:00
draft = false
tags = ["Vulnerability-Management", "CyberSecurity"]
categories = ["CyberSecurity"]
+++

# A Great Interview

I recently just watched [a great interview](https://www.youtube.com/watch?v=be3UD29RIQw) by the VulnWise team talking to my personal mentor and friend, Johnny Shaieb. They were discussing the history of vulnerability management databases and covered a lot of interesting topics like:

* The history of vulnerability databases
* The value of having an unbounded score
* Why clients need to focus on the quality over quantity of vulns
* The history of the fiber backbone that runs the country.
* Etc.

There was one topic though that I felt I'd like to take a crack at. At the 33:42 minute mark of the video Steve Carter or Scott Kuffer asked the question:

```
What do you see as like the biggest gaps today in the technology or the biggest weaknesses and what do you see... in the coming years, changing with regard to the technology side of the house...? 
```

# The 2 biggest problems in IT cybersecurity

The way I see it, there are two REALLY big problems that need to be solved before one can fix the gaps in an organization.

1. Conway's law cannot be broken.

2. Tool Fatigue.

## Conway's Law

Johnny identified this issue, though not by name, in the interview. He mentioned that the team that does onboarding, the team that does asset management, and the team that handles vulnerability management are all different teams who don't talk. This issue is not a technology problem, but a business driven one called Conway's law. Conway's law is the idea that computer systems are laid out architecturally like organizations that make them. 

If you have a relatively small organization where everyone works together in the same office and uses the same tools, you can expect that the IT infrastructure will be pretty monolithic. Everything is in one place and talking because that's what the users need in their clumped environment.

If you have a bigger organization with specialized tribes and departments, you'll often see the tech take a more "micro-services" approach where everything is sequestered into silos. If these organizations don't typically interact with eachother, the technology won't be configured to talk together. 

Allen Holub mentions this same problem in his [Agiltry Course](https://agilitry.com/) when discussing what makes a team agile. His point in one of the videos was that if an organization is organized to be silo'ed, it's really difficult to do anything efficiently, because any time one team finishes their work, it takes a really long time for the next team to pick up the first team's piece and run with it. 

He recommends creating teams that can take a problem or feature from idea to completed using only the members on that team. If for example, you have a front end, a back end, and some middleware, he recommends you have one engineer from each discipline on a single team instead of separate teams that specialize in front end, back end, and middleware.

My thought is that until you manage to create multi-disciplinarian teams at the structural level, you will always have things fall through the cracks; even with fancy tools that are designed to bridge them. And speaking of tools:

## Tool Fatigue

Some food for thought, Which is better: Having a specialized tool for each task or having one mega-tool that does everything?

It pains me to say this as a zealot from the church of the UNIX philosophy, but I'm going to have to say one mega-tool.

I've worked with dozens of clients through the years and one thing that strikes me is that they have like 30 cybersecurity tools that don't talk to eachother. Each uses a proprietary format, gives different results, and each is jockeying to be the "single pane of glass" that you use to interact with your environment.

The problem that I've seen is that for every unique tool you add to the environment:

1. There is a period of inactivity from your team as they learn it.

2. There is a massive, over-inflated technical debt that is taken to make the new data fit in with the current workload.

3. It creates really fragile data pipelines where a change to a single spreadsheet will destroy data normalization.

4. As more tools come together, the previous 2 problems will get more and more aggressive.

Compare that to a feature rich tool like Postgres. I like [this post from the "amazingcto" blog](https://www.amazingcto.com/postgres-for-everything/) because the chart shows how simple you can make your workflow by just normalizing everything on postgres. But even if you don't use postgres, read [this article by Derek Sivers](https://sive.rs/pg) who brings up the following INCREDIBLE arguments:

1. Databases outlive the applications that access them.

2. Simplicity Matters more than we give it credit for.

As companies continue to use more and more tools, their teams will be less and less equipped to use and interpret the data. However, if the teams can focus their attention on 4 or 5 big tools that do a lot more:

1. Training people to use the tools becomes easier.

2. Teams can help eachother learn instead of creating tool silos.

3. Data only has to be normalized among a few nodes.

4. Teams will become familiar with baseline behaviors.

I truly believe that there needs to be a concerted effort to educate CISOs that less is more when it comes to tooling and see if you can show them demonstrably that you can't just buy your way into cybersecurity.

# The Spine: My Proposed solution

If we take Conway's law and Tool Fatigue as the two biggest problems plaguing organizations, then the solution that'll fix this mess will be the concept that can utilize those principles as effectively as possible.

My idea on this is to emulate the human body with our cybersecurity policy.

The human nervous system consists of:

1. A brain which acts as the central hub of decision making.

2. A spine which houses nerve clusters that route connections to the brain.

3. Sensory Neurons which bring data to the brain.

4. Motor Neurons which take data back to the body.

Now, most organizations have something equivalent to points 1, 3, and 4. The NOC, SOC, and ROC's all act as a brain that collects data to make decision. The EDR agents emulate Sensory Neurons, and all the machines act as a company's motor neurons. 

It's the spine that I feel is missing though.

There are central clusers (which I'll call vertebrae) which house all the data flowing to the brain. Until these vertebrae are properly utilized, I feel that there will be significant gaps to cross.

## Vertebrae 1: Firewalls, Routing, and switches

All traffic in a network has to flow through these receptors and it's amazing to me how many companies aren't utilizing IDS/IPS tools like Snort on them. I remember back in school that Zeek/bro, a popular IDS could detect a kali instance by watching for calls to the Offensive Security apt mirrors. Not having at least a passive detection system on these devices will lead to a ton of missing warning shots.

## Vertebrae 2: Active Directory

Hackers eat, breathe, and live AD. I knew a pro in my last job named, Scott Brink. This man could get into the top server of most orgs he tested in like 30 minutes because he understood the most common ways to attack active directory services.

There is a tool called Bloodhound that maps ALL of the relationships in your environment and provides a pretty graph that will tell you the security posture of your AD network. This tool is free. You know how many clients I saw running it on the regular? 0. Until [defenders think in graphs](https://github.com/JohnLaTwC/Shared/blob/master/Defenders%20think%20in%20lists.%20Attackers%20think%20in%20graphs.%20As%20long%20as%20this%20is%20true%2C%20attackers%20win.md) they will consistently get compromised through this super common vector.

## Vertebrae 3: DevOps Tooling

If Active Directory is the King of Windows, then Jenkins and Github are the King of Linux. In the olden days of software development, it could be between weeks to months between various software releases, but these days it can be minutes. This truth has spurred the development of DevOps tools. 

DevOps is an industry term for tools that bridge the gap between developers who create tooling and operations folks who use the tools. The idea behind a DevOps pipeline follows this cycle.

1. Developer pushes code to repo like github.
2. Github will trigger the build platform (like Jenkins)
3. Jenkins will pull the code.
4. Jenkins will run security checks on the code for vulnerabilities or old libraries
5. Jenkins will build the code.
6. Jenkins will test the code.
7. Jenkins will store any artifacts in the right place.
8. Jenkins will pull secrets and api keys into environment variables.
9. Jenkins will use the secrets to deploy things to the cloud.
10. Jenkins will send alerts to the team and open tickets on any failures.
11. Jenkins will alert the operations team what they need to do.

Now the above is just one example of how a DevOps pipeline CAN be used, but what you'll find in reality is just github and a tool like Jenkins or github actions or something just building the code and sending it out.

As you can see, you can build all SORTS of protections into the pipeline. Since Jenkins acts as a one stop shop for all of the other tools, not protecting and hardening it is one of the biggest mistakes you can make.

## Vertebrae 4: The CMDB

A Configuration Management Database, or CMDB, is one of the most important assets of your environment. As one of my genius friends, Keenan Kunzelman, likes to point out:

"Just as improving your communication skills improves every aspect of your life, so too does asset management improve every aspect of your cybersecurity life. And likewise, any deficiency of the CMDB will impact all of your cybersecurity efforts."

What is a CMDB and why does it matter?

It tracks all your users, computers, and their states. It's a one-stop shop for all your environment's data. It should be able to answer the questions:

* What machines do I have?

* What machines are running this business critical software?

* Who owns this machine?

* Who owns this application?

* How do I contact the admin? the data center is flooding!

You can't protect a machine that you don't know exists, and you can't know it exists without a place that tracks machines. 

When you implement business policy, all of it should be routed through your cmdb. Onboarding devices should be a part of your CMDB, as should patching them, as should decommissioning them. If you can't say for sure that everything on your network is logged, you're in for a world of hurt.

This is also a great place to focus automation efforts.

# Conclusion

If you focus on building out your spine, you should have:
1. An understanding of all traffic going through your network
2. An understanding of the battleground for all your Windows Machines
3. An understanding of your most sensitive Linux Machines.
4. A view of all the people and machines in your environment.

With these 4 things in proper order, you should have [80%](https://en.wikipedia.org/wiki/Pareto_distribution) of your cybersecurity posture figured out. The rest will come down to your applications and storage solutions.
