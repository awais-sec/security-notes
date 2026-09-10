# Cloud Computing Fundamentals and Threats

## 1. Introduction to Cloud Computing

Cloud computing is the on-demand delivery of IT capabilities (infrastructure, applications) as a metered service over a network. Examples include Gmail, Facebook, and Dropbox.

**Key characteristics:**

- On-demand self-service: users can provision resources (compute, storage) automatically without human interaction
- Distributed storage: data is stored across multiple locations for better scalability and availability, but can raise security concerns
- Rapid elasticity: resources can be scaled up or down instantly, appearing unlimited to the user
- Automated management: minimizes user involvement, reducing labor costs and human error
- Broad network access: resources are available over the network from various devices (laptops, mobiles)
- Resource pooling: provider's resources are pooled to serve multiple customers in a multi-tenant model
- Measured service: "pay-per-use" model, users are billed based on their consumption of resources
- Virtualization technology: allows for rapid scaling that non-virtualized environments cannot achieve

**Limitations of cloud computing:**

- Limited control and flexibility for organizations
- Prone to outages and technical issues
- Security, privacy, and compliance challenges
- Vendor lock-in and contract issues
- Dependency on internet connectivity
- Vulnerability to attacks since all components are online
- Difficulty migrating between providers
- Potential for high latency and limited bandwidth

## 2. Types of Cloud Computing Services

| Model | Description | Examples | Advantages | Disadvantages |
|---|---|---|---|---|
| IaaS (Infrastructure-as-a-Service) | Provides virtualized computing resources over the internet | Amazon EC2, Microsoft OneDrive | Dynamic scaling, guaranteed uptime, automation, global access | High software security risk, potential performance issues |
| PaaS (Platform-as-a-Service) | Provides a platform to develop, run, and manage applications without managing infrastructure | Google App Engine, Microsoft Azure | Simplified deployment, prebuilt functionality, lower security risk than IaaS, pay-per-use | Vendor lock-in, data privacy concerns, integration challenges |
| SaaS (Software-as-a-Service) | Delivers software applications over the internet on a subscription basis | Google Docs, Salesforce CRM | Low cost, easy administration, global accessibility, high compatibility | Security and latency issues, total dependency on the internet, difficult to switch vendors |
| IDaaS (Identity-as-a-Service) | Identity and access management via a third party (SSO, MFA) | Okta, Microsoft Entra ID | Low cost, improved security, simplified compliance, central user management | Single server failure can disrupt service, vulnerable to account hijacking |
| SECaaS (Security-as-a-Service) | Integrates security services (pen testing, intrusion detection) into corporate infrastructure cost-effectively | eSentire MDR, Foundstone | Low cost, reduced complexity, continuous protection, access to expert security tools | Increased attack surface, unknown risk profile, insecure APIs, no customization |
| CaaS (Container-as-a-Service) | Provides containers and cluster management as a service | Amazon EC2, Google Kubernetes Engine | Streamlined container app development, pay-per-resource, improved security, high scalability | High operational overhead, developer responsible for platform deployment |
| FaaS (Function-as-a-Service) | Runs application functions without managing infrastructure (serverless) | AWS Lambda, Google Cloud Functions | Pay-per-use, low cost, easy deployment, high scalability | High latency, memory limitations, monitoring difficulties, vendor lock-in |
| XaaS (Anything-as-a-Service) | Broad category covering any service delivered over the internet | Salesforce, AWS, Azure | High scalability, location independence, fault tolerance, reduced capital expenditure | Service outages due to internet dependency, performance issues, complex troubleshooting |
| FWaaS (Firewalls-as-a-Service) | Cloud-based firewall that filters network traffic and blocks malicious activity | Zscaler Cloud Firewall, Cisco | Blocks malicious traffic, protects multiple clouds, standardized policies, improved visibility | Resistance to acceptance, network latency |
| DaaS (Desktop-as-a-Service) | Virtual desktops and apps on-demand | Amazon WorkSpaces, Azure Virtual Desktop | Global access, simplified management, reduced downtime, low cost | Security issues, network connectivity problems, high licensing costs |
| MBaaS (Mobile Backend-as-a-Service) | Backend services for mobile apps (user management, databases) via API/SDK | Google Firebase, AWS Amplify | Improved development efficiency, flexibility, scalability, pay-as-you-go | Security issues, high initial costs |

## 3. Separation of Responsibilities in Cloud

Responsibilities are shared between the subscriber and the service provider to prevent conflicts, fraud, and errors. The division depends on the service model (IaaS, PaaS, SaaS). Essentially, the provider manages the underlying infrastructure, while the subscriber is responsible for their data, applications, and identity management.

## 4. OWASP Top 10 Cloud Security Risks

1. **Accountability and Data Ownership** — using public cloud can lead to loss of control and data accountability, risking data recoverability
2. **User Identity Federation** — managing multiple user identities across different cloud providers is complex
3. **Regulatory Compliance** — different laws in different countries create complexity; data secure in one country may not be in another
4. **Business Continuity and Resiliency** — risk of monetary loss if the cloud provider handles business continuity improperly
5. **User Privacy and Secondary Usage of Data** — personal data on social sites is mined for secondary use; default sharing jeopardizes privacy
6. **Service and Data Integration** — unsecured data in transit is susceptible to eavesdropping and interception
7. **Multi-Tenancy and Physical Security** — poor logical segregation between tenants can lead to interference with each other's security
8. **Incident Analysis and Forensic Support** — distributed logs across cloud data centers in different jurisdictions complicate forensic investigations
9. **Infrastructure Security** — misconfiguration may allow network scanning for vulnerabilities like unused ports and default passwords
10. **Non-Production Environment Exposure** — using development/test environments increases the risk of unauthorized access and data modification

## 5. Cloud Computing Threats

- Data breach/loss
- Abuse of cloud services (e.g. password cracking, DDoS)
- Insecure interfaces and APIs
- Insufficient due diligence on the CSP
- Shared technology issues (lack of isolation in multi-tenant environments)
- Unknown risk profile (lack of visibility into CSP's security practices)
- Unsynchronized system clocks (affects log analysis and transactions)
- Inadequate infrastructure design
- Conflicts between client security procedures and the cloud environment
- Loss of operational and security logs
- Malicious insiders
- Illegal access to cloud systems
- Loss of business reputation due to co-tenant activities
- Privilege escalation
- Natural disasters and hardware failure
- Supply chain failure
- Modifying network traffic
- Isolation failure
- Cloud provider acquisition
- Management interface compromise
- Authentication attacks
- VM-level attacks
- Lock-in (inability to migrate)
- Licensing risks
- Loss of governance
- Loss of encryption keys
- Risks from changes of jurisdiction
- Malicious probes or scans
- Theft of equipment
- Cloud service termination
- Subpoena and e-discovery
- Improper data handling and disposal
- Loss/modification of backup data
- Compliance risks
- Economic Denial of Service (EDoS)
- Lack of security architecture
- Hijacking of accounts

## 6. Cloud Computing Attacks

| Attack | Description |
|---|---|
| Service Hijacking (Social Engineering) | Phishing or trickery used to steal credentials and hijack cloud services |
| Service Hijacking (Network Sniffing) | Intercepting unencrypted network traffic to capture sensitive data like passwords |
| Side-Channel Attack | Placing a malicious VM on the same physical host as a target to extract secrets through shared resources (e.g. processor cache) |
| Wrapping Attack | Attacker duplicates and manipulates SOAP message bodies during TLS translation to bypass authentication and run malicious code |
| Man-in-the-Cloud (MITC) | Abuses cloud sync services (e.g. Dropbox) using stolen synchronization tokens for data theft and remote access |
| Cloud Hopper | Targets Managed Service Providers (MSPs) to gain access to their and their customers' intellectual property |
| Cloud Cryptojacking | Unauthorized use of cloud resources to mine cryptocurrency |
| Cloudborne | Implants a persistent backdoor in the firmware of a bare-metal cloud server |
| Instance Metadata Service (IMDS) Attack | Exploits the metadata service to gain credentials and access cloud resources |
| CPDoS / CDN Cache Poisoning | Tricks a web server into sending error pages that get cached by a CDN, causing a denial-of-service for users |
| Cloud Snooper | Bypasses firewalls in AWS security groups to compromise a server |
| Golden SAML | Forges SAML tokens to impersonate users and gain unauthorized access in cloud networks |
| Session Hijacking (XSS) | Steals session cookies via malicious scripts injected into a website |
| Session Hijacking (Session Riding / CSRF) | Tricks a logged-in user into executing unauthorized commands on a web application |
| DNS Attacks | DNS poisoning (redirecting to spoofed sites), cybersquatting, domain hijacking, domain shipping |
| SQL Injection | Inserting malicious code into a database query to access or manipulate confidential data |
| Cryptanalysis Attacks | Breaking weak or flawed encryption to read encrypted cloud data |
| DoS / DDoS | Overwhelming cloud resources to make services unavailable to legitimate users |
| Man-in-the-Browser | Malware in a web browser steals login credentials as they are entered |
| Metadata Spoofing | Modifying cloud service metadata (WSDL files) to redirect users to malicious locations |
| Cloud Malware Injection | Injecting a malicious service or VM into the cloud infrastructure to eavesdrop or steal data |

---
*My notes from the Cloud Forensics module.*
