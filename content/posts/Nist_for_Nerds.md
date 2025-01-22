+++
title = 'Nist for Nerds'
date = 2025-01-21T19:19:20-07:00
draft = false
tags = ["CyberSecurity"]
categories = ["NIST"]
+++

# Introduction:

The National Institute of Standards and Technology (NIST) is a government agency in the US that set up various standards for emerging technologies. In my past job, we used the NIST Cybersecurity Framework (CSF) 2.0 to secure our clients, and I think it might be good to write down some of the principles in case it comes up in a job interview.

This will be a part of a new series called "X for Nerds", where I cover various topics that you might need to know before walking into a cybersecurity job. 

# A breakdown

The NIST CSF is broken down into a few sections.

1. The Core: Which comprises Cybersecurity functions, categories (breakdowns of functions), and subcategories (breakdowns of categories).

2. The Profiles: Which covers Current and Target goals for each of the core function's categories. Think of this as your organizational roadmap.

3. The Tiers: The tiers are something of a maturity model. They cover how well equipped an organization is to tackling threats to the core 6.

# The 5 Core Functions of v1.1

In my past life, we used both v1.1 and v2.0. In this section, I'm going to cover the original 5 functions with the understanding that I'll return to the v2.0 functions next. The 5 core functions of NIST's CSF are:

1. Identify - This function is where you handle all of your asset management. The purpose of "Identify" is to log and categorize ALL of the assets in your environment. Assets can be hardware devices like Laptops, Servers, Printers, and more, but they also encompass the software you use, like email services, spreadsheet tools, device management tools and more. It is important that you log all the versions and identifiers so you know what is in-date and more importantly, what's end-of-life.

2. Protect - The Protect function focuses on all the tools and processes that you have in place to shelter your identified assets from harm. Tools in this case can be as specific as Cisco Umbrella or Microsoft Defender, but it also covers more general technologies like Multi-factor Authentication.

It should be noted that version 2.0 also added a few important categories to protect like platform security and Identity Access Management.

3. Detect - The Detect function covers your intrusion detection and prevention game. Do you have a baseline of security for your environment? Are you monitoring for anomolies? What about physical security? Do you have CCTV cameras, access management logs, locks on sensitive centers?

4. Respond - The Respond function is how well you can react to a cyber attack. How is your business analyzing and assessing the threat? How well can your team contain the damage? Does your team have a response plan with a thorough chain of command and procedures? Is this plan practiced?

5. Recover- The Recover function is in place to restore any capabilities or services that were "impaired" due to a cybersecurity event. Do you have backups that you can restore from? If you utilize a disaster recovery cold-site, how long does it take to make it operational? The Recover function also covers post-mortems. After you've had a cybersecurity event, stakeholders need to be gathered to discuss the event. What plans should be enacted to prevent a future event? These details need to be communicated clearly and effectively.


# The Govern Function

The original NIST CSF v1.1 covered the 5 functions above. In 2024, a sixth function was added to consolidate some of the categories held in the original five by including a governance function. Governance covers things like understanding organizational context, setting up a risk management strategy, setting out Roles, Responsibilities, and authorities, codifying policies, providing oversight, and most importantly, properly managing risk for the Global Supply Chain. 

The inclusion of this function also marks a departure from v1.1 in that cybersecurity was now a responsibility of the governance layer of the organization instead of just the IT department. It's often said that if the C-suite doesn't have buy-in, then it just won't happen. That's what this change addresses.

# The Profiles:

Now that you've gotten the core principles, each of them is broken into a little over 100 categories. These categories are gathered together into a document called a profile. A list of some example profile docs can be found [here](https://www.nccoe.nist.gov/examples-community-profiles). Your business has two of these profiles:

* The first profile is the "Current Profile" and it is an assessment of how your organization currently stacks up to the core functions and each category. It'll most likely uncover a number of points where your organization can improve its cybersecurity posture.

* The second profile is the "Target Profile", and it is a target or goal for your organization to shoot for. Think of it as a picture of your organization following best practices.

Now that you have these two profiles, you'll need to conduct a gap assessment between those two profiles to see where the discrepencies are. This will give you a number of categories to focus on when making your plans.

# The 4 Tiers

Ok, so you've identified where you are and where you need to be. Next is you're going to need to establish your maturity through the tiers.

1. The Partial implementation tier indicates that there are little or no processes in place and your organization is primarily reactive to threats it only moderately understands.

2. The Risk Informed implementation tier implies that the organization understands the threats and risks that they need to respond to, but don't have any policies or procedures in place. The organization is still primary reactive.

3. The Repeatable implementation tier covers an organization that has tools, policies, and procedures to deal with threats in a repeatable manner, but isn't mature enough to handle incidents in real time. It is typically an organization that is very rigid with its procedures and can't adapt to changes very quickly or easily.

4. The Adaptive Implementation tier covers an organization that has matured and has Policies, procedures, tools, and strategies to adapt to cybersecurity incidents in real-time. They have the fingers on the pulse of the organization and know what to do when something happens.

These are the essentials of NIST's CSF. I hope it was informative. As is always the case with cybersecurity practices, this is a continuous practice instead of a "one-and-done".
