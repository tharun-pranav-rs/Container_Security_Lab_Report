\# 🐳 Container Security \& Web Penetration Testing Lab



\## 📖 Project Overview

This project simulates a full \*\*DevSecOps lifecycle\*\*, bridging the gap between offensive security (Red Teaming) and defensive hardening (Blue Teaming). 



I deployed a vulnerable containerized application (\*\*OWASP Juice Shop\*\*) to conduct manual penetration testing, followed by implementing container security best practices to harden the runtime environment against real-world attacks.



\### 🎯 Key Objectives

\* \*\*Assess:\*\* Conduct Dynamic Application Security Testing (DAST) to exploit OWASP Top 10 vulnerabilities.

\* \*\*Analyze:\*\* Perform Software Composition Analysis (SCA) to identify supply chain risks in container images.

\* \*\*Harden:\*\* Implement "Immutable Infrastructure" principles to neutralize persistence attacks.



---



\## 🚩 Phase 1: Offensive Operations (Red Team)

I successfully identified and exploited the following vulnerabilities:

\* \*\*💉 SQL Injection (SQLi):\*\* Bypassed authentication mechanisms to gain Administrative access.

\* \*\*🌐 Cross-Site Scripting (DOM XSS):\*\* Injected malicious scripts into the application DOM via unsanitized input fields.

\* \*\*📂 Sensitive Data Exposure:\*\* Exploited misconfigured Directory Listings to retrieve confidential business documents (`acquisitions.md`).

\* \*\*🔓 Broken Access Control:\*\* Used "Forced Browsing" techniques to access hidden administrative dashboards.



---



\## 🛡️ Phase 2: Defensive Operations (Blue Team)

After exploiting the application, I shifted to a DevSecOps role to secure the infrastructure:



\### 1. Supply Chain Security (SCA)

\* \*\*Tool:\*\* `docker scout`

\* \*\*Findings:\*\* Identified \*\*9 Critical\*\* and \*\*24 High\*\* CVEs within the Node.js application layer, validating the need for automated CI/CD gating.



\### 2. Runtime Hardening (Immutable Infrastructure)

\* \*\*Strategy:\*\* Read-Only Filesystem

\* \*\*Implementation:\*\* Deployed containers with the `--read-only` flag and restricted write access to specific `tmpfs` mounts.

\* \*\*Result:\*\* Successfully blocked "file-dropping" attacks (e.g., webshells, malware persistence) by freezing the container's root filesystem.



```bash

\# Hardened Deployment Command

docker run -d --name secure-web --read-only --tmpfs /tmp nginx

🛠️ Tools \& Technologies

Container Runtime: Docker Desktop



Vulnerability Scanner: Docker Scout



Target: OWASP Juice Shop



Techniques: SQLi, XSS, Forced Browsing, Container Hardening



📄 Full Lab Report

For a detailed technical breakdown of step-by-step exploits and remediation logs, please view the Full Container Security Lab Report.



Author: Tharun Pranav

