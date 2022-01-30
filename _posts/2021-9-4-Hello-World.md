---
layout: post
title: EPP, EDR, XDR, SIEM, UBA, CIA, NSA, WTF?
---

Before I start, I wanted to give you full disclosure that I have built and currently manage a multi-tenant SIEM platform deployed across hundreds of clients, so I can't really claim to be unbiased on this subject. That being said, I started writing this article as a way to better understand the differences between SIEM and XDR, with the hopes of being able to justify to our marketing team that the product's name should be rebranded from SIEM to XDR. I created tables, drew venn diagrams highlighting the commonalities and differences between the two product types. But the more I read, and the more I drew, the clearer it became that other than some negligable differences, the two categories are essentially the same, with the latter being more limited in scope, and a clever marketing response to EDR technology, rather than a new category of technical security control.


Here's how the conversation usually goes for me.

- So what is an XDR?
- It's like an EDR but it also monitors your network grear and cloud platforms.
- Monitor how?
- Ingests things like logs, process info, and some flow data, then puts them all together is some nice dashboards and runs threat detection rules against them. It's actually more like a combination of EDR, NTA, sometimes UEBA, maybe EPP, maybe not, also SEG logs...
- But then how is that different from a SIEM?

How indeed? To answer that question, let's go back in time and review how each of these acronyms was born.

## Endpoint Security: From Antivirus to EPP and EDR
I'm going to spare you the history of computer virus going all the way back to the 70's; you can find a great overview of that on [this](https://en.wikipedia.org/wiki/Computer_virus) wikipedia page. Let's fast forward to the 80's when the first **Antivirus** programs were created. These were your good old Dr. Solomon's Antivirus Toolkit, Norton Antivirus, McAfee, and so on. Their main job was to scan your hard drive for known-bad pieces of malware, mainly by comparing file hashes to a local database of definitions, looking for certain strings in file data, and some heuristic analyasis.

As time went by, a lot of these Antivirus solutions started adding some additional features, namely Host Firewall, Device Control, and Web Traffic Filtering. Subsequently, the name changed from Antivirus or **Endpoint Protection**, abbreviated to EPP. These were your Symantec Endpoint Protection, ESET, WebRoot, etc. Further down the road, companies like Cylance came up with some great static analysis and machine learning solutions, propelling the industry toward **NextGen** Endpoint Protection. Most legacy EPP vendors followed suite and added ML functionalities of their own.

Then came **CarbonBlack** with the valid claim that many malware-less attacks can go right past your NextGen EPP solution if you're not looking at process-related suspicious activity. Examples of these are Living Off the Land techniques such as using Powershell scripts, strange process trees (WinWord.exe spawning cmd.exe), processes running from the Recycle Bin, and so on. This was the birth of **Endpoint Detection and Response**, or EDR technology. The core idea here is that there is a grey area where categorically blocking the activity could wreak havoc on you systems, and you'll end up with very unhappy developers and sysadmins. EDR's main goal was to detect the suspicious activity and report it to your IT team or dedicated SOC for further investigation.

Over the next few years, the lines between EPP and EDR began to get blurry. EDR makers such as CarbonBlack and CrowdStrike started adding more EPP features such as static analysis, signature scans, device control, and firewall management, while EPP vendors such as Symantec and Cylance started to build more EDR-like toolset into their products.

## Network Security: Firewalls, IPS, IDS, and NTA
Now let's shift our focus away from endpoints and zoom in on network traffic. Until recently, most people used to work in an office, using workstation plugged into the corporate network, and all their traffic traversing through the perimeter firewalls. Traditional firewalls such as Cisco PIX, Cisco ASA, or older Fortinet FortiGate firewalls couldn't see beyond layer 4, which means that you had no control over ingress or egress traffic beyond IP addresses and port numbers. Technologies like deep-packet-inspection and SSL decryption are computationally expensive, so larger organizations would rely on **Network Intrusion Prevention Systems (NIPS)** or out-of-band **Network Intrusion Detection Systems (NIDS)** solutions to offline this workload to a dedicated appliance. The problem with most of these technologies was that they required physical connectivity to every firewall and core switch in order to fully capture the data. As a result, **Netowrk Traffic Analysis** solutions that relied on technologies such as NetFlow or erSPAN were devised. Over time, companies like Palo Alto managed to successfully converge the NIPS featureset into their devices, creating what we know as **NextGen Firewalls**. The advent of NextGen Firewalls, and the increase in remote work and cloud applications essentially brought upon the death of NIDS/NIPS solutions, but NTA is still alive and kicking in many organizations.

## SIEM: The Battle for Single Page of Glass 
The idea of log aggregation, parsing and analysis is nothing new in IT; but SIEM didn't really become a *thing* until late 2000's. The idea here is that you ship the logs from everything we already discussed above (EPP, EDR, NTA, etc.), as well as OS and application logs, through a set of ingest pipelines that parse, enrich, and denormalize the data, to a central repository where they can be searched against and retained for a relatively long period of time. Once the data is in the SIEM, you can then use a combination of static rules or ML jobs to look for anomalous activity on your endpoints, applications, network devices, e-mail system, and so on. Consequently, your SOC team no longer has to get bombarded by e-mails alerts from a plethora of different technologies, or log in to dozens of different dashboards for doing investigations. They'll have one place to go: the SIEM.

First generation SIEM solutions were more focused towards log retention for compliance reasons, rather than incident detection and response. Modern SIEM solutions such as the one offered by Elastic, offer hundreds of pre-built detection rules, dozens of dashboards, event correlation functionality (see EQL), and machine learning jobs. Furthermore, you can install EDR-like agents on your endpoints to capture not only event logs, but also process and file informatino using tools such as Sysmon or Auditbeat. You can also leverage NTA functionality by levering syslog, NetFlow, or even port mirroring to a host running Zeek(bro). You can also use API calls to capture logs from essentially any modern hosted application and cloud platform. My area of expertise is mostly in BELK (Beats, Elasticsearch, Logstash, Kibana), but I know these are all possible on Splunk and other major SIEM solutions.

Some organizations further automate their incident reponse by integrating their SIEM into a **SOAR (Security Orchestration, Automation, and Response)** tool. These tools either simplify or automate certain aspects of SOC response, such as using SIEM alerts to blacklist an attacker's IP address on the perimeter firewall, create a DNS sinkhole, or isolate an endpoint.

## So what's XDR?
The term **eXtended Detection and Response (XDR)** was coined by Nir Zuk of Palo Alto Networks in a keynote speech back in 2018. 

Let's start with Gartner's definiton.

> “a SaaS-based, vendor-specific, security threat detection and incident response tool that natively integrates multiple security products into a cohesive security operations system that unifies all licensed components.”

Sounds familiar? If you remove "SaaS-based" and "vendor-specific" from the description above, the rest of that statement describes a subset of what any SIEM tool already provides out of the box. Granted, some (not all) SIEM solutions are not SaaS-based, and SIEM is generally vendor-agnostic, but does any of that really warrant coming up with yet another acronym and further complicating matters for engineers and business professionals alike?

I keep finding articles claiming that SIEM is more of a compliance-tool, better suited for longer log retention, where as XDR is a more advanced approach to detection and response, more aligned with security use cases. But as I explained above, modern SIEM tools utilize a slew of different detection methods (static rules, thresholds, and ML jobs)...



|               | EPP                 | EDR                    | NDR            | XDR          | SIEM             | SOAR |
| ---           | ---                 | ---                    | ---            | ---          | ---              | ---  |
| Workstations  | **AM**, *DC*, *HFW* | **HIDS**, **PM**, *AM* |                | *PM*, *LC*   | *LC*, *LR*       |      |
| Servers       | **AM**, *HFW*       | **HIDS**, **PM**, *AM* |                | **PM**, *LC* | **LC**, **LR**   |      |
| Network Nodes |                     |                        | **NIDS**, *LC* | HIPS, LC     | LC, LR   |      |
| Applications  |                     |                        |                |  LC         | LC, LR   |      |
| Cloud         |                     |          |          |  LC         | LC, LR   |      |
| SIEM          |                     |          |          |             |          | AR   |
