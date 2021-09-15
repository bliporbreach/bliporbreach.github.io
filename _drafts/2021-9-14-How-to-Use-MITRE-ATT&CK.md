---
layout: post
title: How to Use MITRE ATT&CK for Threat Detection
---

So you finally got the budget approval for that advanced detection tool you've been nagging about for years - be it a proprietary SIEM solution (Splunk, Qradar, LogRhythm, etc.), an open-source one like Elastic stack, or an EDR tool like CarbonBlack Response. You spent weeks setting up the environment, clicking through all the menus, reading all the docs, and finally managed to ingest logs from all your firewalls, routers, switches, cloud platforms, endpoint security solution, and applications (IIS, DNS, DHCP) across the environment. You installed agents on all workstations and servers, Windows, Linux, macOS, and began to ingest not only logs but process names, file hashes, commandline arguments, everything under the sun! You used existing parsers or wrote new ones, created gorgeous visualizations and dashboards, complete with histograms and world maps, fully interactive. 

Congratulations! You've done it! You have the data, you have THE POWER!

![He-man - I have the power](https://www.jobnimbus.com/wp-content/uploads/2013/10/tumblr_m336glqeNC1r04pibo1_500.jpg)

You sit back, pour yourself a nice single malt scotch, and admire the view on your screen, riding the waves of those smooth line graphs, staring at those dazzling pastel colored pie charts, ... life. is. beauitful!

... and that lasts a couple of hours. Ok, a few days. Weeks maybe? Then comes the real question: **this is cute and all, but how is any of this making me more secure?**

Enter MITRE ATT&CK!

## MITRE WHAT? ##
There are plenty of articles about MITRE ATT&CK, and their websites does a really good job of describing what it is. A grossly oversimplified way that I describe it to my clients is "MITRE ATT&CK is like the perodic table of all the bad things that bad people can do to your data and systems". *Side note - good people are also capatable of doing very bad things but let's not open that can of worms right now.*  

I'm a firm believer in learning through examples, so let's do some pretend play here. You're the head of cybersecurity at a large health insurance company (my condolences), you deal with all sorts of Personal Health Information (PHI) and Personal Identifiable Infomration (PII) on a daily basis (hence my condolences), and you're worried about getting hacked! You build the beatiful fancy ecosystem I described above, and now you want to get your team to start adding detection rules so you can catch the bad people trying to steal your customers' SSNs and health records. There are dozens of different techniques, and hundreds of procedures, that can be used to infiltrate, evade detection, escalate privilges, move laterally, and exfiltrate data, so where do you start? Realistically thinking, it's impossible to cover them all! New techniques and procedures are discovered every day (ok maybe every week), people can get really creative when it gets to mischief, but if you have to prioritize between the techniques used by a) a penetration tester; b) an attacker targeting your company specificaily; and c) a highly sophisticated group of attackers that genearlly taret health insurance companies; which one would you choose? I tend to go for 'C' (remember, when providing options, just like food you should always save the best (the baklava) for the last) and yes I just used nested paranthesis, I code, sue me. 
