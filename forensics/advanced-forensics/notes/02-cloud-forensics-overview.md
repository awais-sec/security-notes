# Cloud Forensics Overview

## 1. Introduction to Cloud Forensics

Cloud forensics is the application of digital forensic investigation processes in the cloud computing environment. It is considered a subset of network forensics, dealing with investigations in both private and public networks. The procedures vary depending on the cloud computing service and deployment model.

### 1.1 Module Objectives

- Summarize cloud computing concepts.
- List cloud computing attacks.
- Understand the importance of cloud forensics.
- Interpret the usage of cloud forensics.
- Distinguish between various types of cloud forensics.
- Understand the roles of stakeholders in cloud forensics.
- Interpret challenges faced by investigators in cloud forensics.
- Investigate cloud storage services like Dropbox and Google Drive.

## 2. Cloud Computing Overview

Cloud computing provides on-demand delivery of IT capabilities, where infrastructure and applications are offered as metered services over a network.

### 2.1 Characteristics of Cloud Computing

- On-demand self-service
- Distributed storage
- Rapid elasticity
- Automated management
- Broad network access
- Resource pooling
- Measured service
- Virtualization technology

### 2.2 Types of Cloud Computing Services

- **Infrastructure-as-a-Service (IaaS)**: Provides virtual machines and abstracted hardware/operating systems controlled via a service API. Examples: Amazon EC2, GoGrid, SunGrid, Windows SkyDrive.
- **Platform-as-a-Service (PaaS)**: Offers development tools, configuration management, and deployment platforms for custom application development. Examples: Intel MashMaker, Google App Engine, Force.com, Microsoft Azure.
- **Software-as-a-Service (SaaS)**: Delivers software on-demand over the internet. Examples: Google Docs, Google Calendar, Salesforce CRM.

### 2.3 Cloud Deployment Models

- **Private Cloud**: Operates solely for a single organization.
- **Hybrid Cloud**: Combines attributes of multiple cloud types (private, community, or public).
- **Community Cloud**: Shared infrastructure for organizations with common concerns (e.g., security, compliance).
- **Public Cloud**: Services are available over a public network.

## 3. Cloud Computing Threats

1. Data breach/loss
2. Abuse of cloud services
3. Insecure interfaces and APIs
4. Insufficient due diligence
5. Shared technology issues
6. Unknown risk profile
7. Inadequate infrastructure design and planning
8. Conflicts between client hardening and cloud environment
9. Loss of operational and security logs
10. Malicious insiders
11. Illegal access to cloud systems
12. Privilege escalation
13. Loss of business reputation due to co-tenant activities
14. Natural disasters
15. Hardware failure
16. Supply chain failure
17. Modifying network traffic
18. Isolation failure
19. Cloud provider acquisition
20. Management interface compromise
21. Network management failure
22. Authentication attacks
23. VM-level attacks
24. Lock-in
25. Licensing risks
26. Loss of governance
27. Loss of encryption keys
28. Risks from changes of jurisdiction
29. Undertaking malicious probes or scans
30. Theft of computer equipment
31. Cloud service termination or failure
32. Subpoena and e-discovery
33. Improper data handling and disposal
34. Loss or modification of backup data
35. Compliance risks
36. Economic Denial of Sustainability (EDoS)

## 4. Cloud Computing Attacks

1. Service hijacking using social engineering
2. Session hijacking using XSS attacks
3. Domain Name System (DNS) attacks
4. SQL injection attacks
5. Wrapping attack
6. Service hijacking using network sniffing
7. Session hijacking using session riding
8. Side channel attacks or cross-guest VM breaches
9. Cryptanalysis attacks
10. Denial of Service (DoS) and Distributed Denial of Service (DDoS) attacks

See [cloud-data-breach-and-loss.md](03-cloud-data-breach-and-loss.md) and [cloud-service-hijacking-social-engineering.md](04-cloud-service-hijacking-social-engineering.md) for expanded notes on threat #1 and attack #1 above.

## 5. Cloud Forensics Procedures

Cloud forensics procedures differ based on service and deployment models:

- **SaaS and PaaS**: Offer restricted control over process/network monitoring.
- **IaaS**: Allows acquisition of VM instances for evidence analysis.
- **Private Cloud**: Physical access to data is available.
- **Public Cloud**: Physical access is restricted, and data collection relies on the Cloud Service Provider (CSP).

## 6. Usage of Cloud Forensics

- **Investigation**: Probes organized cybercrime, policy violations, and suspicious activities in the cloud.
- **Troubleshooting**: Resolves functional, operational, and security issues.
- **Log Monitoring**: Gathers, examines, and correlates log entries for auditing and compliance.
- **Data and System Recovery**: Recovers deleted or encrypted data/systems post-attack.
- **Due Diligence/Regulatory Compliance**: Ensures data security, record-keeping, and notification of affected parties.

## 7. Cloud Crimes

Cloud crimes are categorized based on the role of the cloud:

- **Cloud as a Subject**: Crimes occur within the cloud environment, e.g., identity theft of cloud user accounts.
- **Cloud as an Object**: The CSP is the target, e.g., DDoS attacks on cloud infrastructure.
- **Cloud as a Tool**: The cloud is used to plan or execute crimes, e.g., storing or sharing crime-related evidence.

## 8. Case Studies

### 8.1 Cloud as a Subject: Man-in-the-Cloud Attacks (August 2015)

- Services like Google Drive, Dropbox, and Microsoft OneDrive are vulnerable to man-in-the-cloud (MITC) attacks.
- Hackers exploit authentication tokens to access data or inject malware/ransomware without needing credentials.
- Attackers use tools like Switcher via malicious email attachments or drive-by downloads.
- MITC attacks are stealthy, using encrypted channels, and may require account deletion to mitigate.
- Source: Imperva report, Black Hat conference.

### 8.2 Cloud as an Object: iCloud Brute Force Attack (January 2015)

- The iDict tool exploited a flaw in iCloud's security, bypassing account lockout restrictions and two-factor authentication.
- Allowed brute force password guessing, targeting the `loginDelegates` functionality.
- Required the email address associated with the iCloud account.
- Apple patched the vulnerability post-discovery.
- Source: Business Insider, SC Magazine UK.

## 9. Cloud Forensics Stakeholders

- **Cloud Service Provider (CSP)**: Manages infrastructure and provides data access.
- **Client**: The entity using cloud services, responsible for data security.
- **Third Parties**: May include outsourced service providers, complicating investigations.
- **Law Enforcement**: Conducts evidence collection and legal proceedings.
- **Academic Researchers**: Contribute to forensic methodologies and training.

## 10. Artifacts Left by the Google Drive Client on Windows

Uninstalling the Google Drive client:

- Removes the client config folder (`sync_config.db`).
- `sync_log.log` entries are recoverable from unallocated space.
- Does not delete local file copies.
- Preserves Prefetch files post-uninstallation.

Additional recoverable information:

- Registry keys of recent files
- LINK files
- Browser history and cache
- Thumbnails
- Registry Point/Volume Shadow Copies
- `Pagefile.sys`
- `Hiberfil.sys`

## 11. Cloud Forensics Tools

### 11.1 UFED Cloud Analyzer

- A tool for extracting and analyzing cloud-based data.
- Supports forensic investigations of services like Dropbox and Google Drive.
- Facilitates evidence collection from cloud storage and applications.
