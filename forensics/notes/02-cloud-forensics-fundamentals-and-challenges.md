# Cloud Forensics: Fundamentals and Challenges

## 1. Introduction to Cloud Forensics

- Cloud forensics is the application of the digital forensic investigation process in a cloud computing environment
- It is considered a subset of network forensics, which deals with investigations in both private and public networks
- Forensic procedures vary significantly depending on the cloud service and deployment model:
  - SaaS and PaaS models provide restricted control over process or network monitoring
  - IaaS models allow for the acquisition of VM instances from the customer for evidence analysis
  - Private clouds may allow physical access to data, while public clouds restrict it

## 2. Uses of Cloud Forensics

- **Investigation** — organized cyber crime, policy violations, and suspicious activities within the cloud
- **Troubleshooting** — resolving functional, operational, and security issues in the cloud ecosystem
- **Log monitoring** — collecting, examining, and correlating log entries across multiple cloud endpoints; assists in auditing, due diligence, and regulatory compliance
- **Data and system recovery** — recovering deleted or encrypted data and restoring systems after damage or attacks
- **Due diligence / regulatory compliance** — helping organizations follow due diligence and adhere to requirements such as securing critical data, maintaining audit records, and notifying parties affected by data exposure

## 3. Cyber Crime in a Cloud Environment

A crime where the cloud is a subject, object, or tool is classified as a cloud crime:

- **Cloud as a Subject** — the crime is committed within the cloud environment (e.g. stealing the identity of cloud user accounts)
- **Cloud as an Object** — the Cloud Service Provider (CSP) is the target of the crime (e.g. DDoS attacks targeting sections of or the entire cloud)
- **Cloud as a Tool** — the cloud is used to plan and commit a crime (e.g. using cloud resources to attack other systems, or storing/sharing crime-related evidence in the cloud)

## 4. Stakeholders and Their Roles

A cloud forensic investigation minimally involves the CSP and the client. The scope expands if the CSP outsources its services to third parties. The stakeholder ecosystem includes:

- Academic (research, education, training)
- Third parties
- Audience/compliance
- Law enforcement (evidence collection, prosecution, confiscation)
- CSP (service legal agreement)
- Customers
- Investigators
- Incident handlers
- Cloud organization (IT professionals, law advisors)

This forms a complex "chain of cloud service providers/customers."

## 5. Cloud Forensics Challenges (NIST Categorization)

Challenges are categorized based on the NIST Cloud Computing Forensic Science Challenges framework (see `NIST.IR.8006.pdf` in the reference folder).

### A. Architecture and Identification

1. Data deletion in the cloud
2. Difficulty recovering overwritten data
3. Interoperability issues among different CSPs
4. Presence of single points of failure (in infrastructure)
5. No single point of failure for criminals to exploit
6. Difficulty detecting malicious acts
7. Criminals' access to low-cost, high computing power
8. Real-time investigation intelligence processes are not possible
9. Malicious code may circumvent VM isolation methods
10. Evidence spread across multiple venues and geolocations
11. Lack of transparency from CSPs
12. Difficulty locating criminal activity within the cloud
13. Challenges with cloud confiscation and resource seizure
14. Errors in cloud-management portal configurations
15. Difficulty segregating potential evidence
16. Unclear boundaries of responsibility
17. Ensuring secure provenance (history of data)
18. Maintaining a verifiable data chain of custody

### B. Data Collection

1. Decreased access and control over data for investigators
2. Complex chain of dependencies between services
3. Difficulty locating evidence
4. Uncertainty about the physical data location
5. Challenges with imaging and isolating data
6. Data is often available for a limited time
7. Difficulty locating the physical storage media
8. General evidence identification problems
9. Data is stored in dynamic storage systems
10. Necessity of live forensics (investigating running systems)
11. Obstruction caused by resource abstraction (virtualization)
12. Application details are often not available to investigators
13. Additional data collection is often not possible after the fact
14. Specific challenges with creating cloud images
15. Problems with selective data acquisition
16. Complexities of cryptographic key management
17. Ambiguous trust boundaries between client and provider
18. Difficulties in ensuring data integrity and evidence preservation
19. Establishing a root of trust for evidence

### C. Logs

- Decentralization of logs across many systems
- Difficulty correlating evidence from disparate logs
- Timestamp synchronization issues across systems
- Challenges in the use of metadata
- Difficulty in the recognition and interpretation of logs
- Complexity due to multiple layers and tiers in the cloud architecture
- Logs may have less evident value compared to traditional forensics

### D. Legal

1. Missing critical terms in contracts or SLAs
2. Limited investigative power of external bodies
3. Heavy reliance on cloud providers for cooperation
4. Uncertainty about the physical data location
5. Issues with port protection for data extraction
6. Challenges with data transfer protocols
7. Complexities of e-discovery in the cloud
8. Lack of international agreements and laws
9. Complications from international cloud services
10. Jurisdictional disputes
11. Challenges with international communication during investigations
12. Conflicts with confidentiality and PII (personally identifiable information) protection laws
13. Reputation fate sharing (one tenant's actions affecting others)

### E. Role Management and Other Challenges

- Difficulty identifying the true account owner
- Lack of universal standards
- Teaching and scientific principles for cloud forensics are not fully addressed
- Limited knowledge of available logs and records
- Use of fictitious identities by criminals
- Lack of standard forensic processes and models
- Need for specialized cloud training for investigators
- Decoupling of user credentials from physical location
- Use of anti-forensics techniques to mislead investigators
- Challenges for incident first responders
- Issues with authentication and access control during an investigation
- Concerns over the competence and trustworthiness of all parties involved

---
*My notes from the Cloud Forensics module.*
