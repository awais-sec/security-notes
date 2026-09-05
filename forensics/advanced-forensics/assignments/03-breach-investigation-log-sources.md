# Assignment #3 — Log Sources for a Breach Investigation

*Advanced Forensics, presented to Kaukab Jamal Zuberi. Submitted by Awais Ahmed, BS-DFCS, Sp-2023, Sec-A, Roll No. 008.*

What I'd examine in a case like this, with a focus on identifying the cause, method, and impact of the breach:

## 1. Firewall Logs

**Why they're critical:** these are the first line of defense and one of the best indicators of ingress and egress traffic. Look for:

- Unauthorized inbound connections (e.g., from foreign IPs)
- Outbound connections to suspicious domains (possible C2 channels)
- Port scans or protocol anomalies

**Artifacts to extract:** source/destination IPs, timestamps, action taken (allowed/denied), protocol/port data.

## 2. Web Server Logs (e.g., Apache, Nginx, IIS)

**Why they're critical:** web applications run portals and APIs, prime targets for initial compromise via web shells, SQLi, or RCE. These logs show malformed or unexpected HTTP requests, POST requests to unauthorized endpoints, and access to sensitive URIs.

**Artifacts to extract:** IP addresses, user agents (check for toolkits like `curl`, `sqlmap`, etc.), request methods and parameters, HTTP status codes.

## 3. Endpoint Detection and Response (EDR) Logs

**Why they're critical:** these provide real-time insight into what's happening at the endpoint level — process execution, memory activity, file changes.

**Key indicators:** suspicious process chains (e.g., `powershell` spawned by `winword.exe`), credential dumping attempts, registry tampering, persistence mechanisms.

## 4. Authentication Logs (Active Directory, RADIUS, LDAP)

**Why they're critical:** most attackers want persistence and lateral movement, and AD logs are a goldmine for credential brute-force attempts, logins at odd hours, and lateral movement via pass-the-hash or ticketing attacks (e.g., Kerberoasting).

**Artifacts to watch:** Event ID 4624 (successful login), 4625 (failed login), 4672 (privileged login), 4768/4769 (Kerberos ticket events).

## 5. VPN and Remote Access Logs

**Why they're critical:** if attackers gained access via stolen credentials, it often happens over VPN. These logs can tie usernames to source IPs and login behavior.

**Look for:** logins from unexpected geo-locations, session duration anomalies, rapid switching between IPs (VPN hopping).

## 6. DNS Logs

**Why they're critical:** attackers often use DNS tunneling or beaconing domains. DNS logs help spot unusual domain queries (long subdomains or frequent requests), domain generation algorithm (DGA) activity, and C2 server lookups.

## 7. Windows Security Event Logs

**Why they're critical:** these give a detailed view into user actions, privilege escalation, and persistence on Windows hosts.

**Important Event IDs:** 4688 (process creation), 4697 (service installed), 7045 (new service installed), 1102 (audit logs cleared — an indicator of cover-up).

## 8. SIEM Aggregated Logs

**Why they're critical:** a well-tuned SIEM (like Splunk, QRadar, Sentinel) centralizes and correlates logs across the enterprise, revealing patterns of attack, timeline building across multiple systems, alert correlation, and detection of MITRE ATT&CK techniques.

## 9. Email Logs (Exchange, O365, Google Workspace)

**Why they're critical:** phishing is still the #1 initial access vector, especially in financial organizations.

**Look for:** malicious attachments or links, SPF/DKIM/DMARC failures, external-to-internal communication anomalies.

## 10. Database Access Logs (e.g., Oracle, MySQL, MSSQL)

**Why they're critical:** if the crown jewels — customer data, transaction records — were touched, these logs can prove it.

**Look for:** unauthorized queries (e.g., `SELECT * FROM customers`), access outside business hours, data dumps or unusually large responses.

## Bonus Tip

Don't forget cloud provider logs if the website owner uses services like AWS CloudTrail, Azure Activity Logs, or GCP Audit Logs. They capture key actions like IAM changes, API calls, and service misuse.
