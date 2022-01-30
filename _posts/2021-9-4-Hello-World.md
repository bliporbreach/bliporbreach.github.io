---
layout: post
title: Test
---

So this is how this coversation usually goes.

- So what is an XDR?
- It's like an EDR but it also monitors your network grear and cloud platforms.
- Monitor how?
- Ingests things like logs, process info, and some flow data, then puts them all together is some nice dashboards and runs threat detection rules against them. It's like a combination of EDR, NTA, UEBA, maybe EPP, maybe not, and sometimes e-mail data...
- But then how is that different from a SIEM?

How indeed? 

|               | EPP                 | EDR                    | NDR            | XDR          | SIEM             | SOAR |
| ---           | ---                 | ---                    | ---            | ---          | ---              | ---  |
| Workstations  | **AM**, *DC*, *HFW* | **HIDS**, **PM**, *AM* |                | *PM*, *LC*   | *LC*, *LR*       |      |
| Servers       | **AM**, *HFW*       | **HIDS**, **PM**, *AM* |                | **PM**, *LC* | **LC**, **LR**   |      |
| Network Nodes |                     |                        | **NIDS**, *LC* |  HIPS, LC     | LC, LR   |      |
| Applications  |                     |          |          |  LC         | LC, LR   |      |
| Cloud         |                     |          |          |  LC         | LC, LR   |      |
| SIEM          |                     |          |          |             |          | AR   |
