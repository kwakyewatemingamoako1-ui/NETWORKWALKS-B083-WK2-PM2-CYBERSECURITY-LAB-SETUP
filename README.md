# $\color{red}{\text{NETWORKWALKS-B083-WK2-PM2-CYBERSECURITY-LAB-SETUP}}$

| **SECURITY ASSESSMENT DOCUMENTATION** | **MODULE: PHASES 1 & 2** |
| --- | --- |
| 🛡️ **PENETRATION TESTING REPORT** | 🔍 **FOOTPRINTING & NETWORK SCANNING PHASES** |

---

> ### 📌 **Document Overview**
> 
> 
> * **Context:** Comprehensive security evaluation and reconnaissance tracking.
> * **Focus Areas:** Information gathering, footprint analysis, active host discovery, and port enumeration.
> 
>
---

🛠️ Completed Modules & Tooling
* W2-PM1 — Multiple Kali Tools (Reconnaissance, enumeration, and footprinting utility execution)

* W2-PM5 — Zenmap Scanning (Advanced network mapping, host discovery, and service port auditing)
# Project Scope & Target Authorization

> **Status Notice:** All activities conducted within this repository and against the specified targets are authorized, legal, and bound by explicit written permission.

## 🎯 Target Scope

| Target | Permission Status | Authorized Phases |
| --- | --- | --- |
| **Networkwalks** | ✅ Secured (Written Permission) | Phases 1 – 5 |
| **Local LAN Network** | ✅ Owned / Authorized | Phases 1 – 5 |

---

## 🚀 Execution Phases

* **Phase 1: Reconnaissance & Footprinting** — Gathering open-source intelligence, mapping the perimeter, and identifying digital footprints. *(Completed)*
* **Phase 2: Scanning & Network Discovery** — Identifying live hosts, open ports, running services, and potential network topologies. *(Completed)*
* **Phase 3–5: Exploitation, Post-Exploitation & Reporting** — *Currently In Progress.* Assessing vulnerabilities, establishing controlled access, and documenting findings.

---

> **Disclaimer:** This repository is maintained for authorized security assessments and educational tracking purposes only. Unauthorized access or scanning of targets without explicit prior consent is strictly prohibited.

## 📝 Introduction

This report documents the reconnaissance and enumeration activities conducted during **Week 2** of my ongoing internship program at Networkwalks. 

This documentation covers two core modules:
* **W2-PM1:** Footprinting the `networkwalks.com` domain utilizing multiple Kali Linux utilities.
* **W2-PM5:** Scanning and mapping my own local laboratory network using Zenmap.

Together, these modules demonstrate the transition from passive intelligence gathering to active host discovery and network mapping.

### 💻 Execution Environment & Documentation Format
* **Environment:** Footprinting was executed within **Kali Linux**, while network scanning was performed from a Windows host running **Zenmap**.
* **Structure:** Each documented step includes the exact command used, the observed output, visual screenshot evidence, and a security note detailing the significance of the finding from an attacker's perspective.

## 🔍 4. Activities Performed

### 4.1 Footprinting & Reconnaissance

I performed reconnaissance against the `networkwalks.com` domain using six Kali Linux tools: WHOIS, WhatWeb, Nslookup, Curl, Wafw00f, and DNSRecon. Each tool was utilized to collect a distinct category of intelligence regarding the target infrastructure.

*   **WHOIS:** Used to obtain publicly available domain registration details and identify the domain's designated name servers, mapping out registrar data and hosting infrastructure.
  
*   **WhatWeb:** Deployed to fingerprint technologies powering the target website. The scan successfully identified **WordPress 7.0.4** and **WP Download Manager 3.3.58**, alongside other exposed application details.
*   **Nslookup:** Utilized to resolve the target domain name to its corresponding IP address, successfully identifying `192.232.216.135`.
*   **Curl:** Executed with the `-I` option to fetch and inspect HTTP response headers. This revealed details about the web application and exposed the active **WordPress REST API** endpoint (`/wp-json/`).
*   **Wafw00f:** Run to detect whether a Web Application Firewall (WAF) was actively protecting the web server. The tool identified **ModSecurity (SpiderLabs)**.
*   **DNSRecon:** Employed to systematically enumerate DNS records, gathering comprehensive data on name servers, mail servers, SPF/TXT records, service records, and underlying DNS server software versions.

### 4.2 Network Scanning with Zenmap

I used Zenmap to discover live hosts on my local network. Since I was connected via my phone's personal hotspot rather than a standard home router, my network utilized a more compact address range than the default example provided in the practical guidelines.

*   **Step 1 — Identify Local IP and Subnet:** 
    My Wi-Fi adapter configuration revealed an IPv4 address of `172.20.10.9` paired with a subnet mask of `255.255.255.240`. 
*   **Subnet Analysis:** 
    Unlike a standard `255.255.255.0` mask (`/24`), this `/28` mask limits the network to 16 total IP addresses. I adjusted my scanning target range accordingly to fit this restricted subnet.
    
    
