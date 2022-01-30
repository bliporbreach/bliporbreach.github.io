---
layout: post
title: EPP, EDR, XDR, SIEM, UBA, CIA, NSA, WTF?
---

So this is how this coversation usually goes.

- So what is an XDR?
- It's like an EDR but it also monitors your network grear and cloud platforms.
- Monitor how?
- Ingests things like logs, process info, and some flow data, then puts them all together is some nice dashboards and runs threat detection rules against them. It's actually more like a combination of EDR, NTA, sometimes UEBA, maybe EPP, maybe not, also SEG logs...
- But then how is that different from a SIEM?

How indeed? To answer that question, let's go back in time and review how each of these acronyms was born.

I'm going to spare you the history of computer virus going all the way back to the 70's; you can find a great overview of that on [this](https://en.wikipedia.org/wiki/Computer_virus) wikipedia page. Let's fast forward to the 80's when the first **Antivirus** programs were created. These were your good old Dr. Solomon's Antivirus Toolkit, Norton Antivirus, McAfee, and so on. Their main job was to scan your hard drive for known-bad pieces of malware, mainly by comparing file hashes to a local database of definitions, looking for certain strings in file data, and some heuristic analyasis.

As time went by, a lot of these Antivirus solutions started adding some additional features, namely Host Firewall, Device Control, and Web Traffic Filtering. Subsequently, the name changed from Antivirus or **Endpoint Protection**, abbreviated to EPP. These were your Symantec Endpoint Protection, ESET, WebRoot, etc. Further down the road, companies like Cylance came up with some great static analysis and machine learning solutions, propelling the industry toward **NextGen** Endpoint Protection. Most legacy EPP vendors followed suite and added ML functionalities of their own.

Then came **CarbonBlack** with the valid claim that many malware-less attacks can go right past your NextGen EPP solution if you're not looking at process-related suspicious activity. Examples of these are Living Off the Land techniques such as using Powershell scripts, strange process trees (WinWord.exe spawning cmd.exe), processes running from the Recycle Bin, and so on. This was the birth of **Endpoint Detection and Response**, or EDR technology. The core idea here is that there is a grey area where categorically blocking the activity could wreak havoc on you systems, and you'll end up with very unhappy developers and sysadmins. EDR's main goal was to detect the suspicious activity and report it to your IT team or dedicated SOC for further investigation.

Over the next few years, the lines between EPP and EDR began to get blurry. EDR makers such as CarbonBlack and CrowdStrike started adding more EPP features such as static analysis, signature scans, device control, and firewall management, while EPP vendors such as Symantec and Cylance started to build more EDR-like toolset into their products.

Now let's shift our focus away from endpoints and zoom in on network traffic. Until recently, most people used to work in an office, using workstation plugged into the corporate network, and all their traffic traversing through the perimeter firewalls. Traditional firewalls such as Cisco PIX, Cisco ASA, or older Fortinet FortiGate firewalls couldn't see beyond layer 4, which means that you had no control over ingress or egress traffic beyond IP addresses and port numbers. Technologies like deep-packet-inspection and SSL decryption are computationally expensive, so larger organizations would rely on **Network Intrusion Prevention Systems (NIPS)** or out-of-band **Network Intrusion Detection Systems (NIDS)** solutions to offline this workload to a dedicated appliance. The problem with most of these technologies was that they required physical connectivity to every firewall and core switch in order to fully capture the data. As a result, **Netowrk Traffic Analysis** solutions that relied on technologies such as NetFlow or erSPAN were devised. Over time, companies like Palo Alto managed to successfully converge the NIPS featureset into their devices, creating what we know as **NextGen Firewalls**. The advent of NextGen Firewalls, and the increase in remote work and cloud applications essentially brought upon the death of NIDS/NIPS solutions, but NTA is still alive and kicking in many organizations.








|               | EPP                 | EDR                    | NDR            | XDR          | SIEM             | SOAR |
| ---           | ---                 | ---                    | ---            | ---          | ---              | ---  |
| Workstations  | **AM**, *DC*, *HFW* | **HIDS**, **PM**, *AM* |                | *PM*, *LC*   | *LC*, *LR*       |      |
| Servers       | **AM**, *HFW*       | **HIDS**, **PM**, *AM* |                | **PM**, *LC* | **LC**, **LR**   |      |
| Network Nodes |                     |                        | **NIDS**, *LC* | HIPS, LC     | LC, LR   |      |
| Applications  |                     |                        |                |  LC         | LC, LR   |      |
| Cloud         |                     |          |          |  LC         | LC, LR   |      |
| SIEM          |                     |          |          |             |          | AR   |
