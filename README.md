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

# Step 1

*   **Whois:**

 Used to obtain publicly available domain registration details and identify the domain's designated name servers, mapping out registrar data and hosting infrastructure.

<img width="1920" height="922" alt="Screenshot_2026-09-16_17_20_21" src="https://github.com/user-attachments/assets/4713a829-8bbc-4908-80dd-026212b34d1e" />

### Tool Findings & Analysis

*   **Registrar:** GoDaddy.com, LLC
*   **Registration Timeline:** Registered on November 6, 2019, with an expiration date of November 6, 2027.
*   **Name Servers:** `NS6135/NS6136.HOSTGATOR.COM` and `NS29/NS30.DOMAINCONTROL.COM` (indicating hosting via HostGator).
*   **Registrant Identity:** Privacy-protected via Domains By Proxy, LLC (Tempe, Arizona).
*   **Abuse Contact:** `abuse@godaddy.com`

### Attacker perspective & Utility:

Name servers immediately reveal the underlying hosting provider. While registrant privacy successfully obscures the real asset owner, the registrar and abuse contact details remain useful for tracking domain lifecycles or planning targeted social engineering vectors.

<br>

# Step 2

* **WhatWeb:**

Deployed to fingerprint technologies powering the target website. The scan successfully identified **WordPress 7.0.4** and **WP Download Manager 3.3.58**, alongside other exposed application details.

<img width="1920" height="922" alt="Screenshot_2026-09-16_19_01_21" src="https://github.com/user-attachments/assets/ae38a2bc-f628-4b8c-832c-41b5430f9933" />

#### Tool Findings & Analysis

*   **Content Management System (CMS):** WordPress 7.1 with plugin **WP Download Manager 3.3.58**.
*   **Web Server & IP:** Apache running on IP address `192.232.216.135`.
*   **Technology Stack:** Bootstrap 7.1, jQuery 3.7.1, HTML5, and Google Tag Manager.

### Attacker Perspective & Utility: 
  
 Pinpointing exact core and plugin versions enables an attacker to instantly query vulnerability databases (like CVE databases) to identify known, unpatched flaws targeting that specific software stack.

<br>

# Step 3

*   **Nslookup:**

Utilized to resolve the target domain name to its corresponding IP address, successfully identifying `192.232.216.135`.

<img width="1920" height="922" alt="Screenshot_2026-09-16_20_07_02" src="https://github.com/user-attachments/assets/8c69c383-e9b5-4263-a76a-5d3ae6f9440e" />

#### Tool Findings & Analysis

*   **Resolved IP Address:** `192.232.216.135` (queried via Google public DNS server `8.8.8.8`).

### Attacker Perspective & Utility:  
    
Translates human-readable domain names into precise numeric IP addresses, establishing the foundation for direct network scanning, port enumeration, and infrastructure mapping.

<br>

# Step 4

*   **Curl:**

Executed with the `-I` option to fetch and inspect HTTP response headers. This revealed details about the web application and exposed the active **WordPress REST API** endpoint (`/wp-json/`).

<img width="1920" height="922" alt="Screenshot_2026-09-16_20_10_34" src="https://github.com/user-attachments/assets/1c137c05-c36b-441c-bfed-3b54a8231213" />

#### Tool Findings & Analysis

*   **Response Headers:** HTTP/2 200 OK, Server: Apache.
*   **Exposed Endpoints & Caching:** WordPress REST API exposed at `/wp-json/`; caching headers identified (`x-nginx-cache`, `x-endurance-cache-level`, indicating an Endurance/HostGator stack).
*   **Cookies:** Sets the `__wpdm_client` cookie (configured with Secure and HttpOnly flags).

### Attacker Perspective & Utility:  
HTTP response headers leak the underlying web server, caching framework, and hidden application endpoints (such as the REST API) without requiring a full page load, providing a prime reconnaissance and attack surface for WordPress environments.
<br><br>

# Step 5

*   **Wafw00f:**

Run to detect whether a Web Application Firewall (WAF) was actively protecting the web server. The tool identified **ModSecurity (SpiderLabs)**.

<img width="1920" height="922" alt="Screenshot_2026-09-16_20_12_37" src="https://github.com/user-attachments/assets/1546d04d-ed2c-45be-bdaf-46f86fcd42f2" />

#### Tool Findings & Analysis

*   **Web Application Firewall:** Detected **ModSecurity (SpiderLabs)**.

### Attacker Perspective & Utility:  
 Confirming the presence of a protective firewall signals that naive, automated attack attempts will likely be blocked or logged. This forces an attacker to slow down, alter their traffic signatures, or attempt evasion and bypass techniques.
<br><br>

# Step 6

*   **DNSRecon:**

Employed to systematically enumerate DNS records, gathering comprehensive data on name servers, mail servers, SPF/TXT records, service records, and underlying DNS server software versions.

<img width="1920" height="922" alt="Screenshot_2026-09-16_20_14_53" src="https://github.com/user-attachments/assets/0da027e3-c95e-4ce7-b64b-17bbfc4692a0" />

#### Tool Findings & Analysis
*   **Mail Server:** `mail.networkwalks.com` resolving to `192.232.216.135`.
*   **DNS Software:** BIND `9.16.23`.
*   **SPF Record:** `v=spf1 +a +mx +ip4:50.87.144.87 include:websitewelcome.com ~all`.
*   **SRV Records:** 8 records identified, all pointing to `_autodiscover._tcp` and mapping to cPanel email hosts (`cpanelemaildiscovery.cpanel.net`), confirming the underlying cPanel environment.

### Attacker Perspective & Utility:  
 Maps out the complete DNS footprint. Every individual record—ranging from mail server targets and underlying DNS software version strings to SPF policies and SRV configurations—reveals crucial architectural setup details and potential structural footholds.
<br><br>

### 4.2 Network Scanning with Zenmap

I used Zenmap to discover live hosts on my local network. Since I was connected via my phone's personal hotspot rather than a standard home router, my network utilized a more compact address range than the default example provided in the practical guidelines.

*   **Step 1 — Identify Local IP and Subnet:** 
    My Wi-Fi adapter configuration revealed an IPv4 address of `172.20.10.9` paired with a subnet mask of `255.255.255.240`. 
*   **Subnet Analysis:** 
    Unlike a standard `255.255.255.0` mask (`/24`), this `/28` mask limits the network to 16 total IP addresses. I adjusted my scanning target range accordingly to fit this restricted subnet.
    
<img width="1896" height="1016" alt="Screenshot 2026-09-17 145355" src="https://github.com/user-attachments/assets/5f69f6d7-949e-4258-b200-dad3406147db" />
    
*   **Scan Results — 2 Live Hosts Found:**
    1. `172.20.10.1` — Mobile hotspot gateway (MAC address: `5A:AD:12:C2:56:64`)
    2. `172.20.10.9` — Local machine / laptop (MAC address: `C0:BF:BE:4D:F0:66`, verified via `ipconfig /all`)

*   **Topology Mapping:** 
    After completing the scan, I navigated to the **Topology** tab within Zenmap, enabled the network legend, and exported the visual diagram as a PDF file for documentation.

  <img width="1892" height="1011" alt="Screenshot 2026-09-17 145438" src="https://github.com/user-attachments/assets/b047f83a-ffd1-48ae-b5a7-ca542dc9f1c3" />

 <br><br>
 
  ## 📊 5. Risk Analysis / Impact

Based on the intelligence gathered during the footprinting and network scanning exercises, I identified the following potential risks and observations:

| # | Risk / Finding | Evidence / Observation | Potential Impact | Risk Level |
| :---: | :--- | :--- | :--- | :---: |
| **1** | Web technology information exposed | WhatWeb identified WordPress and WP Download Manager | Attackers may use exposed version details to identify software requiring security review | 🟠 Medium |
| **2** | Server IP address identifiable | Nslookup resolved the domain to `192.232.216.135` | Provides direct information regarding the network location of the web service | 🟢 Low |
| **3** | HTTP technical information exposed | Curl returned HTTP response headers and exposed `/wp-json/` | May assist in deeper technology fingerprinting and structural enumeration | 🟢 Low |
| **4** | WAF technology identifiable | Wafw00f identified ModSecurity (SpiderLabs) | Reveals specific defensive architecture details about the target web application | 🟢 Low |
| **5** | DNS infrastructure information exposed | DNSRecon identified DNS, mail, and service-related records | Helps build a broader infrastructure and asset profile of the organization | 🟠 Medium |
| **6** | Multiple live hosts visible on local network | Zenmap identified live hosts within the local network segment | Highlights the visibility of connected devices; unknown or unauthorized hosts may be present | 🟠 Medium |

* **Risk Level Key:** ⚫ Critical | 🟠 Medium | 🟢 Low

> **Important Note:** 
> * The risks detailed above represent passive observations derived from footprinting and scanning exercises, **not confirmed vulnerabilities**.
> * These modules focused exclusively on information gathering and host discovery. No active exploitation or vulnerability validation was conducted.
> * Consequently, the exposure of details such as software versions, IP addresses, or DNS records does not inherently indicate that a system is vulnerable. Further authorized security testing would be required to validate any exploitable security flaws.

## 🛡️ 6. Recommendations

Based on the findings and observations gathered during these activities, I recommend implementing the following security improvements:

1. **Review Publicly Exposed Technology Information**  
   Organizations should regularly audit and minimize what software, content management systems (CMS), and plugin details are publicly visible to external observers.

2. **Keep Software Updated**  
   CMS platforms, plugins, and underlying web stack technologies must be patched promptly and cross-referenced against current vendor security advisories.

3. **Review HTTP Headers**  
   HTTP response headers should be fine-tuned or obfuscated to restrict the leakage of unnecessary server versions and internal technical paths (such as REST API endpoints).

4. **Audit DNS Records Regularly**  
   Periodically review public DNS zones to ensure that only required services, mail exchangers, and infrastructure records are exposed to the public internet.

5. **Properly Configure and Monitor the WAF**  
   Keep defensive controls like ModSecurity active and well-tuned, ensuring rules are regularly updated to catch automated scanners and common web attacks.

6. **Perform Regular Internal Network Discovery**  
   Organizations should schedule routine internal network scans to maintain situational awareness of active devices and listening services.

7. **Investigate Unknown Devices**  
   Any unexpected or unrecognized device uncovered during internal network enumeration should be immediately triaged, verified, and accounted for.

8. **Maintain Accurate Network Documentation**  
   Network topologies, asset inventories, and device information must be documented comprehensively and updated regularly to prevent blind spots.

9. **Enforce Authorization Protocols for Security Testing**  
   Ensure that reconnaissance, vulnerability assessment, and penetration testing activities are strictly restricted to systems and networks where explicit written authorization has been secured.
  
## 🏁 7. Conclusion

During Week 2 of my Cybersecurity & Ethical Hacking internship, I successfully completed practical modules focused on footprinting, reconnaissance, and network scanning.

*   **Footprinting & Intelligence Gathering:** By utilizing six distinct Kali Linux tools against the target domain, I gained hands-on experience in mapping digital footprints. I observed how WHOIS exposes domain registration data, WhatWeb fingerprints web technologies, Nslookup handles domain resolution, Curl uncovers hidden HTTP headers and API endpoints, Wafw00f identifies protective defensive controls, and DNSRecon maps out underlying infrastructure records.
*   **Network Discovery:** Through Zenmap, I successfully analyzed my local subnet configuration, identified live hosts, gathered critical IP and MAC address metrics, and generated a structural network topology diagram.
*   **Key Takeaways on Security Assessment:** These exercises reinforced the foundational reality that information gathering is critical to cybersecurity. Long before any active exploitation takes place, an analyst can uncover a substantial amount of operational intelligence simply by analyzing public artifacts and network responses.
*   **The Importance of Documentation:** I also learned that technical findings require clear, structured reporting. A thorough security report must explicitly detail the methodology executed, observations discovered, contextual risks, and actionable recommendations for remediation.
*   **Authorization & Ethics:** Finally, these practical labs underscored the absolute requirement that all reconnaissance and scanning activities must remain bound strictly within authorized scopes and legal parameters.

## 🛠️ Problems Faced & Solutions

### Problem 1: Internet Connectivity Loss After Static IP Configuration

* **The Challenge:**  
  Following the manual configuration of static IPv4 settings, internet connectivity failed or dropped depending on how Kali Linux and NetworkManager handled the network interface initialization (often related to Duplicate Address Detection (DAD) timeouts hanging the connection).

* **The Fix:**  
  I resolved the issue by modifying the NetworkManager profile to bypass or disable the DAD timeout using the following command:
  ```bash
  sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0

After applying this change, I restarted the network connection and verified that internet connectivity was fully restored.


### Problem 2: Non-standard Subnet on My Network

* **The Challenge:**  
  The practical guide assumed a typical home network subnet mask of `255.255.255.0` (a `/24` network containing 256 addresses). However, because my test environment was connected via a mobile hotspot, my actual local network utilized a `255.255.255.240` mask (a `/28` network restricted to just 16 addresses). Blindly copying the guide's default range would have resulted in inaccurate or inefficient scanning parameters.

* **The Fix:**  
  I executed `ipconfig` first to verify my local network's exact adapter configurations rather than assuming it matched the documentation. Using this real-world data, I calculated and targeted the correct subnet range (`172.20.10.0/28`) instead of the standard `/24` block, ensuring my scan accurately covered only the active local address space.

## 👤 Author

**Kwakyewa Teming-Amoako**  
Cybersecurity Professional B083 

LinkedIn:https://www.linkedin.com/public-profile/settings/?trk=d_flagship3_profile_self_view_public_profile&lipi=urn%3Ali%3Apage%3Ad_flagship3_profile_view_base%3BQyMJwNkPRuWS595FE6F%2Fsg%3D%3D

*Networkwalks Cybersecurity Internship Program*

## 📌 Project Information

* **Program Name:** Cybersecurity at Networkwalks
* **Week:** Week 2
* **Project:** Cybersecurity & Pentesting Lab Setup
* **Repository Platform:** GitHub
