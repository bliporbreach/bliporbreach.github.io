---
layout: post
title: How to Use MITRE ATT&CK for Threat Detection
---

So you finally got the budget approval for that advanced detection tool you've been nagging about for years - be it a proprietary SIEM solution (Splunk, Qradar, LogRhythm, etc.), an open-source one like Elastic stack, or an EDR tool like CarbonBlack Response. You spent weeks setting up the environment, clicking through all the menus, reading all the docs, and finally managed to ingest logs from all your firewalls, routers, switches, cloud platforms, endpoint security solution, and applications (IIS, DNS, DHCP) across the environment. You installed agents on all workstations and servers, Windows, Linux, macOS, and began to ingest not only logs but process names, file hashes, commandline arguments, everything under the sun! You used existing parsers or wrote new ones, created gorgeous visualizations and dashboards, complete with histograms and world maps, fully interactive. 

Congratulations! You've done it! You have the data, you have THE POWER!

![He-man - I have the power](https://www.jobnimbus.com/wp-content/uploads/2013/10/tumblr_m336glqeNC1r04pibo1_500.jpg)

You sit back, pour yourself a nice single malt scotch, and admire the view on your screen, riding the waves of those smooth line graphs, staring at those dazzling pastel colored pie charts, ... life. is. beauitful!


... and that lasts a couple of hours. Ok, maybe a few days. Then comes the real question: how is any of this making me more secure?


