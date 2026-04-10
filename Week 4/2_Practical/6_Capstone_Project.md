---
# Capstone Project: Full VAPT Engagement

---

## Objective

To simulate a complete Vulnerability Assessment and Penetration Testing (VAPT) engagement by identifying, exploiting, and documenting vulnerabilities across network and web application layers.

---

## Lab Environment

**Attacker Machine:** Kali Linux (192.168.198.128)  
**Target 1:** Metasploitable2 (192.168.198.130)  
**Target 2:** OWASP BWA (192.168.198.131)  

**Tools Used:** Metasploit, Nmap, Burp Suite, OpenVAS  

---

## Phase 1: Reconnaissance

### Nmap Scan

```bash
nmap -sC -sV 192.168.198.130
````

<p align="center">
  <img src="Screenshots/nmap_scan_msf2.png" width="700"><br>
  <b>Figure 1: Nmap Scan Results (Metasploitable2)</b>
</p>

---

## Phase 2: Exploitation

### VSFTPD Backdoor Exploit

```bash
msfconsole
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.198.130
run
```

### Result

* Remote Code Execution achieved
* Command shell obtained

<p align="center">
  <img src="Screenshots/ms_exploit_vsftpd.png" width="700"><br>
  <b>Figure 2: VSFTPD Exploitation (RCE)</b>
</p>

<p align="center">
  <img src="Screenshots/shell_access.png" width="700"><br>
  <b>Figure 3: Remote Shell Access</b>
</p>

---

## Phase 3: Post-Exploitation

### Privilege Escalation

* Used enumeration techniques (LinPEAS)
* Identified SUID misconfiguration
* Achieved root access

<p align="center">
  <img src="Screenshots/suid_nmap_exploit.png" width="700"><br>
  <b>Figure 4: Privilege Escalation via SUID</b>
</p>

---

## Phase 4: Web/API Testing (OWASP BWA)

### Burp Suite Testing

* Intercepted HTTP requests
* Identified IDOR (BOLA) vulnerability
* Manipulated parameters to access unauthorized data

<p align="center">
  <img src="Screenshots/burp_http_history.png" width="700"><br>
  <b>Figure 5: Burp HTTP Interception</b>
</p>

<p align="center">
  <img src="Screenshots/bola_exploit.png" width="700"><br>
  <b>Figure 6: BOLA Exploitation</b>
</p>

<p align="center">
  <img src="Screenshots/carlos_login_success.png" width="700"><br>
  <b>Figure 7: Unauthorized Access as Carlos</b>
</p>

---

## Attack Timeline Log

| Timestamp           | Target IP       | Vulnerability | PTES Phase        |
| ------------------- | --------------- | ------------- | ----------------- |
| 2026-04-10 14:00:00 | 192.168.198.130 | VSFTPD RCE    | Exploitation      |
| 2026-04-10 14:20:00 | 192.168.198.130 | SUID Priv Esc | Post-Exploitation |
| 2026-04-10 15:00:00 | 192.168.198.131 | IDOR / BOLA   | Web Testing       |
| 2026-04-10 15:30:00 | 192.168.198.130 | Multiple CVEs | Scanning          |

---

## PTES Report

### Executive Summary

This assessment evaluated the security posture of a simulated environment consisting of Metasploitable2 and OWASP Broken Web Applications. The objective was to identify vulnerabilities across network services and web applications. Critical vulnerabilities were identified, including remote code execution via a vulnerable FTP service and insecure direct object references in web applications. These issues could allow attackers to gain unauthorized access, escalate privileges, and extract sensitive data. Immediate remediation is recommended to reduce risk exposure.

---

### Attack Timeline

The engagement began with reconnaissance using Nmap, revealing multiple exposed services. Exploitation was performed using Metasploit, targeting the VSFTPD backdoor to achieve remote code execution. Post-exploitation involved privilege escalation through SUID misconfiguration, resulting in root access. Web application testing using Burp Suite identified IDOR vulnerabilities. Finally, scanning confirmed additional weaknesses and outdated services.

---

### Remediation Plan

* Update vulnerable services (e.g., VSFTPD)
* Remove unnecessary SUID permissions
* Implement strong access controls in web applications
* Validate user input and enforce authentication checks
* Enable regular vulnerability scanning and patch management
* Apply least privilege principle across systems

---

## Stakeholder Briefing

This assessment identified several critical security weaknesses within the tested environment. A vulnerable FTP service allowed attackers to gain remote access to the system, while improper system configurations enabled privilege escalation to administrative levels. Additionally, web application vulnerabilities allowed unauthorized access to sensitive user data.

These issues pose a significant risk to system integrity, confidentiality, and availability. If exploited in a real-world scenario, attackers could take full control of systems and access critical information.

To mitigate these risks, it is recommended to update outdated software, enforce proper access controls, and implement secure coding practices. Regular security assessments and monitoring should also be conducted to detect and prevent future attacks. Addressing these vulnerabilities will significantly improve the overall security posture of the organization.

---

## Conclusion

This capstone demonstrated a complete VAPT lifecycle, from reconnaissance to exploitation, post-exploitation, and reporting. The findings highlight the importance of proactive security measures and continuous monitoring to protect systems from real-world attacks.

---

