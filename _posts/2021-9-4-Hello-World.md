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

How indeed? To answer that question, let's go back in time and review how each of these acronyms came to be.


|               | EPP                 | EDR                    | NDR            | XDR          | SIEM             | SOAR |
| ---           | ---                 | ---                    | ---            | ---          | ---              | ---  |
| Workstations  | **AM**, *DC*, *HFW* | **HIDS**, **PM**, *AM* |                | *PM*, *LC*   | *LC*, *LR*       |      |
| Servers       | **AM**, *HFW*       | **HIDS**, **PM**, *AM* |                | **PM**, *LC* | **LC**, **LR**   |      |
| Network Nodes |                     |                        | **NIDS**, *LC* | HIPS, LC     | LC, LR   |      |
| Applications  |                     |                        |                |  LC         | LC, LR   |      |
| Cloud         |                     |          |          |  LC         | LC, LR   |      |
| SIEM          |                     |          |          |             |          | AR   |
