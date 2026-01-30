# 🧪 Article Series: A Full-Scale Virtual Lab “Like in Production”

Hi 👋

In this article, I want to talk not about a single server or a specific configuration, but about an **entire series of materials** that I will be publishing step by step.

We will gradually build a full-scale virtual laboratory, максимально close to a real corporate infrastructure: networking, domain services, business systems, security, and monitoring.

This is not a set of “quick how-to guides”.  
It’s a deliberate, structured journey — with explanations of decisions, common mistakes, and the skills you actually gain along the way.

Here you’ll find the overall idea, structure, and logic of the entire series.  
If you’re interested not just in repeating steps, but in **understanding what you’re doing and why** — welcome.

---

## What This Lab Includes

This will not be a “click-through and it works” setup, but a **real engineering story**: a virtual infrastructure that closely resembles a corporate environment, including:

- **Segmented network with firewall, NAT, VPN**
- **OPNsense as a perimeter firewall**
- **Multiple MikroTik devices** (core / edge / branch / Wi-Fi / policies)
- **2 Domain Controllers (AD DS) + DNS**
- **File Server**
- **MS SQL**
- **Bitrix24 (self-hosted)**
- **1C**
- **Jira (self-hosted)**
- **Microsoft Exchange**
- and all of this with monitoring, backups, documentation, security, and typical “real-world” problems

---

## Why This Is Interesting

Because this is _almost exactly how a real business infrastructure is built_ — just running at home, in Proxmox, or in the cloud.

And most importantly, you’re not just “learning”.  
You’re **building an engineer’s portfolio**, where every decision can be demonstrated: diagrams, configurations, logic, “why this way”, mistakes, and fixes.

---

## How Many Skills You’ll Gain

A lot — and not abstract ones. You’ll build real, practical expertise in:

- networking fundamentals (**VLAN, routing, ACL, NAT, VPN, DNS**)
- Windows Server (**AD, GPO, FSMO, replication, DNS, NTFS/SMB**)
- security (**segmentation, hardening, least privilege, logging, MFA, bastion concepts**)
- business services (**Jira, Bitrix, 1C, SQL, Exchange**)
- DevOps-style thinking: **backups, updates, configuration management, IaC logic, documentation, disaster recovery**

And here’s the best part:

You won’t just be “watching tutorials”.  
You’ll be **building a system**, where every mistake becomes a lesson with real “production-level” value.

---

# 📚 Series Table of Contents

## 0) Introduction and Lab Rules

- 0.1. What we are building, service roles, overall architecture
- 0.2. Core principles: “like in production” — segmentation, least privilege, backups, logging
- 0.3. Tools: Proxmox, cloud, VLAN-aware bridges, VM/CT templates, snapshots, naming conventions
- 0.4. Documentation: network diagram, IP table, access matrix, change log

---

## 🔌 “Networking First” Block: Fundamentals + Practice

1. **Networking Without Pain: What an Engineer Must Understand**
2. **The OSI Model — Explained Like a Human**
3. **IP Addressing, Subnets, Routing: Hands-On Practice**
4. **VLANs and Segmentation: Escaping the Flat Network**
5. **NAT, Gateways, Firewalls: Why “Internet Works” ≠ “Everything Works”**
6. **DNS as the Foundation of Everything (and Why It Breaks Everything)**
7. **VPN Practice: Roadwarrior + Site-to-Site (OPNsense + MikroTik)**
8. **Wi-Fi and Security: WPA2/WPA3, Guest Networks, Client Isolation**

---

## 🧱 Platform and Perimeter

9. **Proxmox as the Foundation: Networking, Storage, Templates, Backups, Snapshots**
10. **OPNsense: Perimeter Firewall, VLANs, NAT, Rules, VPN, IDS/IPS**
11. **MikroTik #1 (CORE): VLAN Trunks, DHCP Relay, Inter-VLAN Routing (If Needed)**
12. **MikroTik #2 (EDGE/WAN): Topologies, PPPoE/DHCP, Failover**
13. **MikroTik #3 (BRANCH/Wi-Fi/Access): CAPsMAN/AP, Guest VLANs, Policies**

---

## 🏛️ Active Directory and Core Services

14. **DC01: First Domain Controller (AD DS, DNS, Base Structure)**
15. **DC02: Second Domain Controller (Replication, FSMO, High Availability)**
16. **DNS in the Domain: Zones, Forwarders, Split DNS, Common Pitfalls**
17. **GPO: Security and Usability Policies (Must-Have Set)**
18. **File Server: SMB, NTFS, DFS, Quotas, Shadow Copies, Auditing**

---

## 🧩 Business Services

19. **MS SQL Server: Installation, Best Practices, Backups, Permissions, Maintenance**
20. **Bitrix24 Self-Hosted: Deployment, Environment, Security, Updates**
21. **1C: 1C Server, Publishing, SQL Backend, Licensing, Optimization**
22. **Jira Self-Hosted: Installation, Database, LDAP/AD Integration, Mail, Backups**
23. **Exchange: Base Deployment, Certificates, Mail Flow, AD Integration**

---

## 🛡️ Security, Observability, and “Real Life”

24. **Windows Server Hardening: A Practical Baseline**
25. **OPNsense + MikroTik Hardening: What Actually Matters**
26. **Backup Strategy: 3-2-1 Rule, Restore Testing, Scheduling**
27. **Monitoring and Logging (Zabbix / Prometheus + Grafana / ELK — Choosing the Stack)**
28. **PKI and Certificates: Internal CA, TLS for Services, Auto-Renewal**
29. **RBAC and Access Matrix: Who Can Access What and Why**
30. **Failure Scenarios: “DC Down”, “DNS Broken”, “VPN Offline” — How to Fix**
31. **Final Review: Documentation, Diagrams, Checklists, Portfolio Artifacts**

---

# 🧠 Article Breakdown (Content, Engagement, Skills)

Below is a **mini technical brief for each article**: storyline, hands-on steps, common mistakes, and final deliverables.

---

## 1) Networking Without Pain: What an Engineer Must Understand

**Goal:** Build a mental “map of the world” so everything else fits logically.

**Inside:**
- how to think: L2 vs L3, broadcast domains, default gateways
- why “ping doesn’t work” is not magic but a specific failure
- essential tools: `ping`, `traceroute`, `nslookup` / `dig`, `arp`, `netstat`

**Artifacts:** troubleshooting checklist: “no network / no internet / no domain”.  
**Skills:** engineering mindset + troubleshooting.

---

## 2) The OSI Model — Explained Like a Human

**Inside:** each layer mapped to real failures and checks.  
**Highlight:** a table “symptom → layer → check → fix”.  
**Skills:** structured thinking, fast debugging.

---

## 3) IP Addressing, Subnets, Routing: Hands-On Practice

**Inside:** CIDR, subnet calculation, default routes, static routes.  
**Practice:** 3 subnets, 2 routers, route verification.  
**Skills:** foundation for VLANs and VPNs.

---

## 4) VLANs and Segmentation

**Inside:**
- why VLANs matter (security + order)
- trunk vs access, tagged vs untagged, native VLAN (and why it’s dangerous)
- segment design: USERS / SERVERS / MGMT / DMZ / GUEST

**Skills:** network design, security.

---

## 5) NAT, Gateways, Firewalls

**Inside:** NAT vs routing, stateful firewalls, why NAT ≠ security.  
**Practice:** OPNsense rules + outbound NAT.  
**Skills:** confident perimeter management.

---

## 6) DNS as the Foundation of Everything

**Inside:** A / AAAA / CNAME / SRV / PTR, forwarders, split DNS, cache and TTL.  
**Practice:** intentionally breaking and fixing DNS (to remember it forever).  
**Skills:** one of the most valuable infrastructure skills.

---

## 7) VPN: Roadwarrior + Site-to-Site

**Inside:** choosing WireGuard / IPsec / OpenVPN, routing, split tunneling.  
**Practice:** MGMT and SERVERS access, policy control.  
**Skills:** professional remote access design.

---

## 8) Wi-Fi and Security

**Inside:** guest networks, client isolation, VLAN per SSID, RADIUS / 802.1X (future).  
**Skills:** access-layer security.

---

## 9) Proxmox as the Foundation

**Inside:** bridges, VLAN-aware networking, storage, templates, cloud-init, snapshots.  
**Highlight:** “how not to destroy the lab with one click”.  
**Skills:** virtualization as a profession.

---

## 10) OPNsense: Perimeter Firewall

**Inside:**
- interfaces, VLANs, NAT, rules
- aliases, schedules, floating rules
- VPN and basic IDS/IPS (optional)

**Artifacts:** exported configuration + firewall rules diagram.  
**Skills:** core network security.

---

## 11) MikroTik CORE

**Inside:** VLAN trunks, bridge VLAN filtering, DHCP, basic firewall filters.  
**Highlight:** common “enabled vlan filtering and everything died” cases.  
**Skills:** real-world MikroTik administration.

---

## 12) MikroTik EDGE / WAN

**Inside:** WAN topologies, failover, health checks, basic policy routing.  
**Skills:** reliability, production thinking.

---

## 13) MikroTik BRANCH / Wi-Fi / Access

**Inside:** access layer design, CAPsMAN / APs, guest VLANs, restrictions.  
**Skills:** corporate access-layer networking.

---

## 14) DC01: First Domain Controller

**Inside:**
- domain design: name, OUs, groups
- AD DS and DNS deployment
- time sync, NTP, admin protection

**Artifacts:** OU and group structure + responsibility document.  
**Skills:** Windows infrastructure foundations.

---

## 15) DC02: Second Domain Controller

**Inside:**
- replication, Sites & Services
- FSMO roles: placement and reasoning
- failure testing: planned DC01 shutdown

**Skills:** high availability, confidence.

---

## 16) DNS in the Domain

**Inside:** zones, forwarders, scavenging, SRV records, reverse zones.  
**Skills:** DNS stops being scary.

---

## 17) GPO: Must-Have Policies

**Inside:** password policies, UAC, firewall, RDP, auditing, restrictions.  
**Skills:** security and environment control.

---

## 18) File Server

**Inside:** SMB / NTFS permissions, inheritance, AGDLP, quotas, shadow copies, auditing.  
**Highlight:** standard department / project / personal folder models.  
**Skills:** corporate file systems.

---

## 19) MS SQL Server

**Inside:** installation, collation, tempdb, permissions, backups, maintenance plans.  
**Highlight:** “SQL should never run as `sa`”.  
**Skills:** backend foundation for 1C / Jira / Bitrix.

---

## 20) Bitrix24 Self-Hosted

**Inside:** requirements, environment setup, deployment, security, updates, backups.  
**Highlight:** how to keep it from falling apart in a week.  
**Skills:** business application operations.

---

## 23) Exchange

**Inside:** planning, deployment, certificates, mail flow, SPF / DKIM / DMARC (baseline), integration.  
**Highlight:** doing it “properly”, not “just to make it work”.  
**Skills:** enterprise-level infrastructure experience.

---

## 24–31) Security / Backups / Monitoring / Incidents

This is the **final layer** that turns a working lab into an **engineer-grade system**:

- **Hardening** (minimal, but production-ready)
- **3-2-1 backup strategy** + restore testing
- **Monitoring and logs** (detecting issues before users do)
- **PKI** (certificates, TLS, trust)
- **RBAC** (least privilege)
- **Incident scenarios**: analysis using OSI and system logic