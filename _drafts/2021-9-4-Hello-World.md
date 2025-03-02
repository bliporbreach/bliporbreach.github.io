---
layout: post
title: XDR
---

Every now and then, I am approached by coworkers or clients asking questions about the ever-expanding list of security tools (EPP, EDR, NTA, HIPS, HIDS, UBA, UEBA, SIEM, XDR, SASE, etc.), what they mean, and how they differ from one another. I usually manage to navigate my way through the maze fairly easily, giving them a rundown of all the different tools, delving deeper into the evolution and convergence of EPP and EDR tools, explaining where SIEM and MDR fit in, and speculating about the effects of SASE technology on the future of firewalls and Internet security. But when it gets to XDR, things get a bit weird. Is it really a groundbreaking approach to detection and response, big enough to have its own acronym? Or is it where log aggregation and SIEM technology have already been going for more than a decade? Is it a central repository of alerts from different log sources auto-magically correlating and bundling themselves into incidents? Or is there a team of human operators doing all that work like any MDR solution? Is it really the next best thing since sliced bread? A natural evolution for EPP/EDR? Or is it just a smart marketing strategy?

To answer these questions, we need to do quick review of the evolution of detection and response tools across endpoints and IT infrastructure.

## Endpoint Security: From Antivirus to EPP and EDR
I will spare you the story of self-replicating programs and the origins of computer virus going all the way back to the 70's; you can find a great overview of that on Wikipedia [1]. Let's fast forward to the 80's when the first **Antivirus** programs were created. These were the good old Dr. Solomon's Antivirus Toolkit, Norton Antivirus, McAfee Antivirus, and all the other security programs we all grew up with back in the late 80's and early 90's. Their main job was to scan your whopping 10GB hard drive for malware, and they would do this by comparing files against a local database of known-bad hashes, or by looking for certain patterns and strings inside these files, similar to the way we search for IOCs in SIEM logs nowadays. Later on, some vendors added heuristic analysis as a way to detect new variants of viruses without relying on exact signatures and patterns [2].

Over time, the word *malware* replaced virus, becoming an umbrella term covering all different categories of malicious programs: viruses, rootkits, adware, spyware, potentially unwanted programs (PUP), and worms. Around the same time, Antivirus vendors started adding some additional features such as Host Firewall, Device Control, and Web Traffic Filtering. They moved away from the rather narrow and specific term Antivirus, to the broader and more aptly named label **Endpoint Protection (EPP)**. Then sometime around 2016, a company called Cylance shook the industry by releasing a fully AI-driven antivirus solution. There is still some debate on Cylance's testing methodology [3], nevertheless, this move propelled the world toward **NextGen** Endpoint Protection, and most legacy EPP vendors quickly followed suit and added ML functionalities of their own. So from a business and consumer perspective, "behold, it was very good".

Then came startups like CarbonBlack and CrowdStrike, with the valid claim that many malware-less attacks can go right past your EPP solution if you're not looking at process-related suspicious activity. Examples of these are Living Off the Land techniques such as malicious Powershell scripts, strange process chains, executables running from the Recycle Bin, etc. This led to the birth of **Endpoint Detection and Response (EDR)** technology. The core idea here was that there is a grey area where categorically blocking the activity could wreak havoc on your systems, and you'll end up with very unhappy developers and sysadmins. EDR's main goal was to detect the suspicious activity and report it to your IT team or dedicated SOC for further investigation, isolation, and remediation.

Over the next few years, EDR makers started adding more EPP features such as static analysis, signature scans, device control, and firewall management, while EPP vendors began to build more EDR-like tools into their products. This further blurred the lines between EPP and EDR [4]. Today, most endpoint protection startups design products with the full EPP and EDR spectrum from the get-go.

## Network Security: Firewalls, IPS, IDS, and NTA
Now let's shift our focus away from endpoints and look at network infrastructure. Until recently, most employees used to work in brick-and-mortar offices, using workstations plugged into the corporate network, with all their traffic traversing through the perimeter firewalls. Traditional firewalls couldn't see beyond layer 4, which means that you had no control over ingress or egress traffic beyond IP addresses and port numbers. Technologies like deep-packet-inspection and SSL decryption were expensive, both computationally and financially, so larger organizations would rely on dedicated inline **Network Intrusion Prevention Systems (NIPS)** or out-of-band **Network Intrusion Detection Systems (NIDS)** hardware appliances. The problem with most of these technologies was that they required physical connectivity to every firewall or core switch in order to fully capture the data. There were also other problems associated with port mirroring: oversaturation, dropped frames [5], and limitations inherent with using TCP RESET [6] as a way of stopping traffic flow. Over time, companies like Palo Alto managed to successfully converge the NIPS feature set into their devices, creating what we know as **NextGen Firewalls**. The advent of NextGen Firewalls, and the increase in remote work and cloud applications essentially brought upon the end of NIDS/NIPS solutions.

While firewall vendors were busy making improvements to their line of products, a number of security vendors came up with **Network Traffic Analysis** solutions to address the challenges associated with SPAN ports and inline IPS tools. Instead of looking at real frames and packets, NTA technology would rely on traffic *metadata* to detect anomalous activity. This was mainly achieved using Netflow, erSPAN, and Syslog collected remotely from network equipment. The product would then apply static rules, IOCs, or anomaly detection ML jobs to alert the SOC of suspicious activity.

## SIEM: The Elusive Single Page of Glass 
The idea of log aggregation, parsing and analysis is not new in IT; but SIEM didn't really become a *thing* until late 2000's [7]. The idea here was that you can ship logs from operating systems, applications, appliances, and all the tools we discussed above (EPP, EDR, NTA, etc.), pass them through a set of ingest pipelines that parse, enrich, and denormalize the data, then store them in a central repository where they can be searched against and retained for a relatively long period of time. Once the data was in the SIEM, you could use a combination of static rules or ML jobs to look for anomalous activity on your endpoints, applications, network devices, e-mail system, and so on. Consequently, your SOC team no longer has to get bombarded by e-mail alerts from a plethora of different technologies, or log in to dozens of different dashboards to conduct investigations. They would have only one place to go: the SIEM.

First generation SIEM solutions were more focused on log retention and satisfying compliance requirements. The anomaly detection methods were almost an afterthought, falling behind some of the more sophisticated methods using by EDR vendors. However, modern SIEM solutions offer hundreds of pre-built detection rules, dozens of dashboards, event correlation functionality, and machine learning jobs. Furthermore, you can install EDR-like agents on your endpoints to capture not only event logs, but also process and file information using tools such as Sysmon or Auditd. You can also leverage NTA functionality by using syslog, NetFlow, or even mirroring your switch ports to a host running Zeek(bro). For hosted applications and cloud platforms, you would simply make API calls to pull the latest data. My area of expertise is mostly in BELK (Beats, Elasticsearch, Logstash, Kibana), but I know these are all possible on Splunk and other major SIEM solutions.

Some organizations further automate their incident response by integrating their SIEM into a **SOAR (Security Orchestration, Automation, and Response)** tool. These tools either simplify or automate certain aspects of SOC response, such as using SIEM alerts to blacklist an attacker's IP address on the perimeter firewall, create a DNS sinkhole, or isolate an endpoint.

## So what's XDR?

The term **eXtended Detection and Response (XDR)** is relatively new. It was coined by Nir Zuk of Palo Alto Networks in a keynote speech back in 2018 [8]. The easiest way for me to explain this is by describing a hypothetical dialogue between our favorite security experts, Bob and Alice. 

Alice - So what is an XDR?<br>
Bob - It's like an EDR but it also monitors your network gear and cloud platforms.<br>
Alice - So it's kind of like a SIEM?<br>
Bob - Kind of... but SIEM doesn't really focus on endpoints, it doesn't care about running processes or file paths, it just ingests event logs, syslog, etc.<br>
Alice - Depends on the SIEM though... These days most of the modern ones like Splunk, Elastic, and AlienVault have endpoint agent that ingest event logs, syslog, process info, network traffic, and file integrity data, either using built-in modules or by leveraging things like Sysmon, Auditd, osquery, etc.<br>
Bob - True, but they don't have the endpoint protection (EPP) component, so it's all detection only. With XDR, it's detection AND prevention.<br>
Alice - Yeah but SIEM products have evolved in that area too... so it's just a matter of time...<br>
Bob - Yes, but they're not quite there yet. Also, SIEM is better suited for compliance, not detection and response; whereas XDR is better at detecting suspicious activity using ML rules...<br>
Alice - That's really not the case anymore. Most modern SIEM tools have an extensive library of static detection rules and ML jobs these days.<br>
Bob - But they're mostly on-prem, XDR is a SaaS service.<br>
Alice - AlienVault is SaaS, then there's Splunk Cloud and Elastic Cloud, QRadar has a cloud offering too.<br>
Bob - ...<br>
Alice - So what really is the difference between SIEM and XDR?

To answer that question, let's start with Gartner's definition for XDR.

> “a SaaS-based, vendor-specific, security threat detection and incident response tool that natively integrates multiple security products into a cohesive security operations system that unifies all licensed components.”

Sounds familiar? If you remove "vendor-specific" and "licensed" from the description above, the rest of that statement describes a subset of what any SIEM tool already provides out of the box. Does any of that really warrant coming up with yet another acronym, making everyone feel even more confused and miserable?

To further clarify things for myself, I started breaking down the main components of each of each toolset in order to see how much overlap we're dealing with. I have used a bold font for core components, and italic for the additional features I've seen across different products.


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

A quick glance at the table above shows that SIEM and XDR do in fact have a great amount of overlap. Does that make XDR and SIEM the same? No, at least not yet. Even though the two categories are very similar, XDR is still closer to the endpoint since it has its roots in EPP and EDR technology. If you look at the brochures and marketing information of XDR products, almost all of them have a mature antivirus/EDR component, but they integrate with only a handful of infrastructure and cloud platforms. That's nothing compared to the plethora of integration modules offered by the likes of Splunk, Qradar, and Elastic. 

That being said, can you really blame EDR vendors for having a dream? Can you blame them for coming up with a new name that is sexier than SIEM, a term that has sadly become synonymous to "complex" and "costly"? Probably not. But do you think an EPP company can build a parser like Logstash and a platform that can scale to thousands of nodes, store terabytes of data, and be able to run a search for an IOC in your Apache logs going back a year and tell you, within seconds, whether you were ever hit by a zeroday webshell attack that's been just discovered? Or can it parse your multiline custom application log that's not JSON or CSV or CEF, and uses a weird timestamp? I doubt it.

SIEM vendors have been busy too! They have also been trying to push into the Endpoint market. A prime example of this is Elastic. They started with detect-only Beats for things like logs, processes, and network connections, but have recently entered the endpoint protection market by acquiring EndGame and releasing Elastic Agent [9]. But would a SIEM vendor be able to detect a hypervisor rootkit or something that has injected itself into your MBR or UEFI partition? Maybe... who knows!

![SIEM vs. XDR](../images/siem_vs_xdr.jpg)

For me, the takeaway from my research was that the question whether XDR is a marketing play or a unique category in cybersecurity toolset is irrelevant at this point. It is being quickly adopted by the industry as the new sexy thing, regardless of all the overlaps with SIEM technology. I have even seen it being used by SIEM vendors recently in their marketing material. So the term XDR is here to stay, and it will possibly replace SIEM in the future. What remains to be seen is whether the vendors can actually deliver on their promise of painless integration, low noise, better correlation, and more accurate ML jobs.





[1] https://en.wikipedia.org/wiki/Computer_virus, 'Computer Virus'<br>
[2] https://cs.stanford.edu/people/eroberts/cs181/projects/2000-01/viruses/anti-virus.html, 'How Anti-Virus Software Works'<br>
[3] https://arstechnica.com/information-technology/2017/04/the-mystery-of-the-malware-that-wasnt/ 'Lawyers, malware, and money: The antivirus market’s nasty fight over Cylance'<br>
[4] https://www.gartner.com/imagesrv/media-products/pdf/symantec/symantec-1-4SNI36O.pdf?es_p=6816496, 'The Evolution of Endpoint Protection'<br>
[5] https://www.garlandtechnology.com/blog/stop-misusing-span-ports-or-risk-losing-network-traffic-data, 'Stop Misusing SPAN Ports Or Risk Losing Network Traffic Data'<br>
[6] https://www.computerworld.com/article/2521502/tcp-reset--pros-and-cons.html, 'TCP RESET: Pros and Cons'<br>
[7] https://cybersecurity-magazine.com/a-brief-history-of-siem/, 'A Brief History of SIEM'<br>
[8] https://www.stratospherenetworks.com/blog/what-is-xdr-your-guide-to-extended-detection-and-response/, 'What Is XDR? Your Guide to Extended Detection and Response'<br>
[9] https://www.elastic.co/blog/whats-new-elastic-security-7-16-0 'Elastic Security 7.16: Accelerate SecOps with the most powerful Elastic Security yet'<br>
