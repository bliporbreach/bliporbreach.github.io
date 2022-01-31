---
layout: post
title: XDR
---

> Disclosure of potential bias
> 
> While I have made an effort to provide an unbiased account on this subject, the reader must be aware that I have built and currently manage a large SIEM deployment across more than 100 organizations [TBD]

In my line of work, every once in a while I am approached by a coworker or a client with questions about the ever-expanding list of security tool acrnoyms (EPP, EDR, NTA, HIPS, HIDS, UBA, UEBA, SIEM, XDR, etc.), what they mean, and how they differ from one another. Lately, some of the most common questions have been around the differences between SIEM and XDR. Here's how the conversation usually goes:

Alice - So what is an XDR?<br>
Bob - It's like an EDR but it also monitors your network gear and cloud platforms.<br>
Alice - So it's kind of like a SIEM?<br>
Bob - Kind of... but SIEM doesn't really focus on endpoints, it doesn't care about running processes or suspicious behavior, it just ingests logs. <br>
Alice - Depends on the SIEM though, endpoint agents by Splunk, Elastic, AlienVault ingest event logs, syslog, process info, network traffic, and file integrity data, either using built-in modules or by leveraging things like Sysmon, Auditd, osquery, etc.<br>
Bob - True, but they don't have the endpoint protection (EPP) component, so it's all detection only. With XDR, it's detection AND prevention.<br>
Alice - Yeah but I've heard SIEM vendors are ramping up on that area too, Elastic Agents have EPP components for examples... so it's just a matter of time...<br>
Bob - Yeah, but also SIEM is better suited for compliance, not detection and response; whereas XDR is better at detecting suspcious activity using ML rules...<br>
Alice - That's really not the case anymore though, most modern SIEM tools have an extensive library of static detection rules and ML jobs these days.<br>
Bob - But they're mostly on-prem, XDR is a SaaS service.<br>
Alice - AlienVault is SaaS, then there's Splunk Cloud and Elastic Cloud, QRadar has a cloud offering too.<br>
Bob - ...<br>
Alice - So what really is the difference between SIEM and XDR?

To answer that question, we need to do quick review of the evolution of protection, detection, and response tools across endpoints, network nodes, and the cloud. I have tried to cite references as much as possible but the information outlined below is mainly based on my own experience and obervations in this field for the last 20 years. 

## Endpoint Security: From Antivirus to EPP and EDR
I'm going to spare you the history of computer virus going all the way back to the 70's; you can find a great overview of that on [this](https://en.wikipedia.org/wiki/Computer_virus) wikipedia page [1]. Let's fast forward to the 80's when the first **Antivirus** programs were created. These were your good old Dr. Solomon's Antivirus Toolkit, Norton Antivirus, McAfee, and so on. Their main job was to scan your hard drive for malware, mainly by comparing files againsta local database of known-bad hashes or looking for certain strings in file data. Later on, many vendors added some heuristic analyasis to identify suspicious activity similar to what you can get with EDR tools these days [2].

As time went by, a lot of these Antivirus solutions started adding some additional features, namely Host Firewall, Device Control, and Web Traffic Filtering. Subsequently, the name changed from Antivirus or **Endpoint Protection (EPP)**. These were your Symantec Endpoint Protection, ESET, WebRoot, etc. Further down the road, companies like Cylance came up with some great static analysis and machine learning solutions, propelling the industry toward **NextGen** Endpoint Protection. Most legacy EPP vendors quickly followed suit and added ML functionalities of their own.

Then came CarbonBlack and CrowdStrike, with the valid claim that many malware-less attacks can go right past your NextGen EPP solution if you're not looking at process-related suspicious activity. Examples of these are Living Off the Land techniques such as using Powershell scripts, strange process trees (WinWord.exe spawning cmd.exe), processes running from the Recycle Bin, and so on. This led to the birth of **Endpoint Detection and Response (EDR)** technology. The core idea here is that there is a grey area where categorically blocking the activity could wreak havoc on you systems, and you'll end up with very unhappy developers and sysadmins. EDR's main goal was to detect the suspicious activity and report it to your IT team or dedicated SOC for further investigation.

Over the next few years, EDR makers such as CarbonBlack and CrowdStrike started adding more EPP features such as static analysis, signature scans, device control, and firewall management, while EPP vendors such as Symantec and Cylance started to build more EDR-like toolset into their products. This further blurred the lines between EPP and EDR [3]. Today, most endpoint protection startups (e.g. SentinelOne) design the products with the full EPP and EDR spectrum from the get-go.

## Network Security: Firewalls, IPS, IDS, and NTA
Now let's shift our focus away from endpoints and zoom in on network traffic. Until recently, most employees used to work in brick and mortar offices, using workstations plugged into the corporate network, with all their traffic traversing through the perimeter firewalls. Traditional firewalls such as Cisco PIX, Cisco ASA, or older Fortinet FortiGate devices couldn't see beyond layer 4, which means that you had no control over ingress or egress traffic beyond IP addresses and port numbers. Technologies like deep-packet-inspection and SSL decryption were computationally expensive, so larger organizations would rely on **Network Intrusion Prevention Systems (NIPS)** or out-of-band **Network Intrusion Detection Systems (NIDS)** solutions to offload this workload to dedicated appliances. The problem with most of these technologies was that they required physical connectivity to every firewall and core switch in order to fully capture the data. As a result, **Netowrk Traffic Analysis** solutions that relied on technologies such as NetFlow or erSPAN were devised. Over time, companies like Palo Alto managed to successfully converge the NIPS featureset into their devices, creating what we know as **NextGen Firewalls**. The advent of NextGen Firewalls, and the increase in remote work and cloud applications essentially brought upon the death of NIDS/NIPS solutions, but NTA is still alive and kicking in many organizations.

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



|               | EPP                 | EDR                    | NDR            | XDR                      | SIEM                         |
| ---           | ---                 | ---                    | ---            | ---                      | ---                          |
| Endpoints     | **AM**, *DC*, *HFW* | **HIDS**, **PM**, *AM* |                | **AM**, **HIDS**, **PM** | **LC**, **LR**, *HIDS*, *PM* |
| Network Nodes |                     |                        | **NIDS**, *LC* | **NIDS**, *LC*           | **LC**, **LR**, **NIDS**     |
| Applications  |                     |                        |                | *LC*                     | **LC**, **LR**               |
| Cloud         |                     |                        |                | *LC*                     | **LC**, **LR**               |

AM: Anti-Malware<br>
PM: Process Monitoring<br>
DC: Device Control<br>
HFW: Host Firewall<br>
HIDS: Host Intrusion Detection System<br>
NIDS: Network Intrustion Detection System<br>
LC: Log Collection<br>
LR: Log Retention<br>


[1] https://en.wikipedia.org/wiki/Computer_virus, 'Computer Virus'<br>
[2] https://cs.stanford.edu/people/eroberts/cs181/projects/2000-01/viruses/anti-virus.html, 'How Anti-Virus Software Works'<br>
[3] https://www.gartner.com/imagesrv/media-products/pdf/symantec/symantec-1-4SNI36O.pdf?es_p=6816496, 'The Evolution of Endpoint Protection'<br>
