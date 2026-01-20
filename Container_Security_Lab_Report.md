🐳 Container Security \& Web Application Penetration Testing Report

1\. Executive Summary

Date: January 20, 2026 Analyst: Tharun Pranav Subject: Application Security Testing (DAST) \& Container Hardening



Objective: To conduct a comprehensive security assessment of a containerized web application (OWASP Juice Shop) using manual penetration testing techniques to identify critical vulnerabilities. Subsequently, to demonstrate DevSecOps best practices by performing vulnerability scanning (SCA) and implementing immutable infrastructure controls to harden the runtime environment.



2\. Lab Environment

Host System: Windows 11 (WSL2 Backend)



Container Runtime: Docker Desktop



Target Application: OWASP Juice Shop (bkimminich/juice-shop)



Tools Used: Docker CLI, docker scout, Web Browser (Chrome/Edge), Nginx



3\. Phase 1: Web Application Penetration Testing (DAST)

3.1 Authentication Bypass (SQL Injection)

Vulnerability: SQL Injection (SQLi) in Login Form.



Methodology: Injected a standard tautology payload into the email field to manipulate the backend SQL query, forcing the database to evaluate the condition as "True" without a valid password.



Payload: ' or 1=1 --



Result: Successfully bypassed authentication and gained access to the Administrator account (admin@juice-sh.op) with full privileges.



3.2 Access Control Violation (Forced Browsing)

Vulnerability: Security by Obscurity / Broken Access Control.



Methodology: Attempted to access hidden administrative pages by guessing common URL paths not linked in the main navigation.



Target Path: /#/score-board



Result: Successfully accessed the hidden "Score Board" page, revealing application tracking data and solving the challenge.



3.3 Client-Side Attack (DOM XSS)

Vulnerability: DOM-Based Cross-Site Scripting (XSS).



Methodology: Injected malicious HTML code into the application's search bar. The application failed to sanitize user input before rendering it in the Document Object Model (DOM).



Payload: <iframe src="javascript:alert('xss')">



Result: The browser executed the injected JavaScript, triggering a pop-up alert. This demonstrates how an attacker could execute arbitrary scripts to steal session cookies.



3.4 Sensitive Data Exposure

Vulnerability: Directory Listing Enabled / Unprotected Sensitive Files.



Methodology: Navigated to the /ftp directory to check for improper server configuration regarding file indexing.



Result: The server listed all files in the directory. Retrieved a confidential document named acquisitions.md, which contained sensitive business strategy information.



4\. Phase 2: DevSecOps \& Container Security

4.1 Vulnerability Scanning (SCA)

Tool: Docker Scout



Command: docker scout quickview bkimminich/juice-shop



Findings:



Base Image: distroless/static:nonroot (0 Critical Vulnerabilities) - Verified secure OS layer.



Application Layer: Identified 9 Critical and 24 High vulnerabilities within the Node.js dependencies.



Remediation Plan: Update package.json dependencies and implement a CI/CD gate to block builds with Critical CVEs.



4.2 Runtime Hardening (Immutable Infrastructure)

Objective: Prevent unauthorized file modification and malware persistence (e.g., webshells) in a compromised container.



Methodology: Deployed an Nginx container with a Read-Only Filesystem, using tmpfs mounts only for strictly necessary runtime directories (/var/cache, /var/run, /tmp).



Command:



Bash



docker run -d --name secure-web --read-only --tmpfs /tmp nginx

Validation: Attempted to write a file to the root directory: docker exec secure-web touch /hacked.txt



Result: Operation failed with "Read-only file system" error.



Conclusion: The container is immune to file-system based persistence attacks.



5\. Conclusion

This lab demonstrated the full lifecycle of a security engagement. We successfully exploited critical web vulnerabilities (OWASP Top 10) to understand the attacker's perspective. We then pivoted to defense, using container scanning to identify supply chain risks and implementing "Immutable Infrastructure" principles to neutralize potential breaches at the runtime level.

