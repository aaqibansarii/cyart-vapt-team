# VAPT Project – Full Security Assessment (Week 2)

This repository contains a complete **Vulnerability Assessment and Penetration Testing (VAPT)** project performed in a controlled lab environment.

The project demonstrates a full penetration testing lifecycle including reconnaissance, scanning, exploitation, post-exploitation, and reporting.

---

## Project Overview

- **Target Environment:** OWASP BWA / Metasploitable2  
- **Tools Used:** Kali Linux, Nmap, OpenVAS, Metasploit, sqlmap, Maltego  
- **Focus Areas:** Web Security, Network Security, Exploitation, Credential Attacks  

---

## Repository Structure

Repository Structure
Introduction

Week 2/
│
├── 1_Vulnerability_Scanning/
│ └── Vulnerability analysis using Nmap & OpenVAS
│
├── 2_Reconnaissance/
│ └── OSINT, Subdomain Enumeration, Tech Stack Analysis
│
├── 3_Exploitation/
│ └── Exploitation using Metasploit (Tomcat, vsftpd)
│
├── 4_Post_Exploitation/
│ └── Evidence collection, hashing, and system access validation
│
├── 5_Capstone_Project/
│ └── Full VAPT cycle (SQL Injection & Credential Extraction)
│
├── Screenshots/
│ └── All supporting evidence and outputs
│
├── VAPT 02.pdf
│ └── Final report submission
│
└── README.md
└── Project documentation and navigation guide
  
---

## Key Highlights

- ✅ Vulnerability scanning using **Nmap & OpenVAS**  
- ✅ Reconnaissance using **WHOIS, Sublist3r, Maltego, Wappalyzer**  
- ✅ Exploitation of:
  - vsftpd backdoor (RCE)
  - Apache Tomcat (Meterpreter session)  
- ✅ Post-exploitation:
  - Root access verification  
  - Evidence collection  
  - SHA256 hashing for integrity  
- ✅ SQL Injection exploitation using **sqlmap**
- ✅ Database extraction and **password cracking**
- ✅ Full PTES-based reporting  

---

## Capstone Achievement

- SQL Injection successfully exploited on DVWA  
- Database enumerated (`dvwa`)  
- User credentials extracted and cracked  
- Weak hashing (MD5) identified  
- Full application compromise demonstrated  

---

## Screenshots

All proof-of-concept and outputs are stored in:


Week 2/Screenshots/


Includes:
- Nmap & OpenVAS results  
- Exploitation proof  
- SQLmap output  
- Hash verification  
- Recon tools output  

---

## Disclaimer

This project was conducted in a **controlled lab environment** for educational purposes only.  
All testing was performed on intentionally vulnerable systems with proper authorization.

---

## Author

**Mohd Aaqib**  
Cybersecurity Student | Ethical Hacking Enthusiast  

---

## Learning Outcomes

- Understanding of full VAPT lifecycle  
- Hands-on exploitation of real vulnerabilities  
- Practical experience with industry tools  
- Ability to document and report security findings professionally  

---

## Final Note

This project demonstrates real-world penetration testing skills and follows structured methodologie



----
----
# Introduction
---
# Penetration Testing Techniques

### What is Penetration Testing?

Penetration testing is a controlled simulation of real-world cyber attacks to identify, exploit, and assess vulnerabilities in a system. Unlike vulnerability scanning, which only detects weaknesses, penetration testing actively validates them by attempting exploitation.

It provides a realistic understanding of how an attacker can compromise a system and what impact it can have on confidentiality, integrity, and availability.

---
---

## How is Penetration Testing Structured? (PTES-Based Phases)

According to PTES (Penetration Testing Execution Standard), penetration testing follows a well-defined lifecycle:

1. Reconnaissance (Intelligence Gathering)

This phase focuses on collecting as much information as possible about the target.

- Techniques:
  - Passive: WHOIS, DNS records, Google dorking
  - Active: Network probing
- Tools: Shodan, Maltego

Shodan plays a critical role by indexing internet-exposed devices. It allows attackers and testers to identify publicly accessible services like SSH, RDP, or databases.

Purpose:
To map the attack surface before interacting directly with the system.

---

2. Scanning and Enumeration

This phase transitions from passive to active interaction.

- Activities:
  - Port scanning (Nmap)
  - Service/version detection
  - Vulnerability scanning (Nessus, OpenVAS)
  - Enumeration (users, services, shares)

This phase identifies:
- Open ports (entry points)
- Running services (attack vectors)
- Known vulnerabilities (potential exploits)

Deep Insight:
Misconfigured services (e.g., outdated Apache, exposed SMB) often lead to exploitation opportunities.

---

3. Exploitation

In this phase, identified vulnerabilities are actively exploited.

- Tools: Metasploit, custom scripts
- Examples:
  - Exploiting weak credentials
  - SQL Injection to bypass authentication
  - Remote Code Execution vulnerabilities

This phase answers:
“Can this vulnerability actually be used to gain access?”

Important:
Not all vulnerabilities are exploitable. This phase validates real risk.

---

4. Post-Exploitation

Once access is gained, the tester evaluates the extent of compromise.

- Activities:
  - Privilege escalation (user → root/admin)
  - Persistence (maintaining access)
  - Data extraction
  - Lateral movement

Deep Insight:
This phase determines the **impact**, not just the vulnerability.

Example:
A low-privilege shell can become critical if privilege escalation is possible.

---

5. Reporting

This is one of the most critical phases.

- Includes:
  - Vulnerability details
  - Proof of Concept (PoC)
  - Risk level (CVSS)
  - Remediation steps

Reports must be:
- Clear
- Structured
- Actionable

Without proper reporting, even successful exploitation has no practical value.

---

### What are Penetration Testing Methodologies?

Structured methodologies ensure consistency, completeness, and professionalism.

---

PTES (Penetration Testing Execution Standard)

- Covers the full lifecycle:
  - Pre-engagement (scope, rules)
  - Intelligence gathering
  - Threat modeling
  - Exploitation
  - Reporting

Key Strength:
Provides a **complete framework**, not just technical steps.

---
---

##  OWASP WSTG

The OWASP Web Security Testing Guide (WSTG) provides a structured approach specifically for web application penetration testing. It focuses on identifying vulnerabilities across different components such as authentication, session management, input validation, and business logic.

Unlike general frameworks, WSTG is deeply technical and test-case driven, making it highly relevant for web pentesting and bug bounty scenarios.

---

### What are the Phases of Penetration Testing?

While OWASP WSTG is not strictly phase-based like PTES, its testing approach can be mapped to standard penetration testing phases:

1. Reconnaissance (Information Gathering)

- OWASP WSTG Section: Information Gathering (WSTG-INFO)
- Activities:
  - Identify application structure
  - Discover endpoints, subdomains, technologies
- Example:
  - Using Shodan to find exposed services
  - Using tools like Wappalyzer for tech stack identification

Purpose:
To understand the attack surface before interacting with the application.

---

2. Scanning and Enumeration

- OWASP WSTG Sections:
  - Configuration and Deployment Management Testing
  - Identity Management Testing

- Activities:
  - Port scanning (Nmap)
  - Service detection
  - Vulnerability scanning (Nessus, OpenVAS)
  - Enumerating users, directories, APIs

Purpose:
To identify potential weaknesses and entry points in the system.

---

3. Exploitation

- OWASP WSTG Sections:
  - Authentication Testing (WSTG-ATHN)
  - Input Validation Testing (WSTG-INPV)
  - Session Management Testing (WSTG-SESS)

- Activities:
  - SQL Injection
  - Cross-Site Scripting (XSS)
  - Authentication bypass
  - File inclusion attacks

Example:
Using Metasploit or manual payloads to exploit identified vulnerabilities.

Purpose:
To validate whether vulnerabilities can be successfully exploited.

---

4. Post-Exploitation

- Not explicitly defined in WSTG, but inferred from impact analysis

- Activities:
  - Privilege escalation
  - Accessing sensitive data
  - Maintaining session or persistence

Purpose:
To evaluate the impact of a successful attack.

Insight:
OWASP focuses more on identifying vulnerabilities, while PTES extends into deeper post-exploitation analysis.

---

5. Reporting

- OWASP emphasizes proper documentation of findings

- Includes:
  - Vulnerability description
  - Proof of Concept (PoC)
  - Risk severity
  - Remediation steps

Purpose:
To communicate risks clearly to developers and stakeholders.

---

### How do PTES and OWASP WSTG Work Together?

- PTES:
  Provides the full penetration testing lifecycle (from scoping to reporting)

- OWASP WSTG:
  Provides detailed technical testing methods specifically for web applications

Example:
PTES is used to define scope and phases, while OWASP WSTG is used during exploitation to test for vulnerabilities like SQL Injection and XSS.

---


### Key Takeaway

OWASP WSTG provides deep technical guidance for identifying web vulnerabilities, while PTES ensures that the entire penetration testing process is structured, ethical, and complete. Together, they enable effective and professional security assessments.

---
---

## SANS Case Studies (Real-World Insight)

SANS case studies demonstrate how attacks occur in real environments.

- Show:
  - Attack chain (recon → exploit → impact)
  - Mistakes made by organizations
  - Detection and response gaps

Key Insight:
Real attacks rarely rely on a single vulnerability—they combine multiple weaknesses.

---

### What is the Role of Ethics in Penetration Testing?

Ethics are critical and are emphasized in both PTES and OWASP methodologies.

Key Principles:

- Authorization:
  Testing must only be performed with explicit client permission

- Defined Scope:
  Only allowed systems and applications should be tested

- Controlled Execution:
  Avoid actions that may disrupt services or cause damage

- Data Protection:
  Any sensitive data accessed must be handled securely

Insight:
Penetration testing without authorization is illegal and equivalent to malicious hacking.

---

### Key Objectives of Penetration Testing

- Simulate real-world attacks to identify exploitable vulnerabilities
- Validate whether detected vulnerabilities can be abused
- Measure the actual impact of a security breach
- Follow structured methodologies (PTES, OWASP WSTG)
- Provide actionable recommendations for remediation

---

### Analyst Insight

Penetration testing is not just about exploiting vulnerabilities but understanding the full attack path. A minor misconfiguration, when combined with another weakness, can lead to a critical compromise. Structured methodologies help ensure that no attack vector is overlooked.

---
