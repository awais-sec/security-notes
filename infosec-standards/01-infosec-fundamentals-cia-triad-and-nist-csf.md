# INFOSEC Fundamentals, the CIA Triad, and the NIST Cybersecurity Framework

## 1. Introduction to Information Security (INFOSEC)

**Definition and Core Framework:** international and federal Information Security (INFOSEC) standards and regulations are essential frameworks designed to protect information and information systems from unauthorized access, use, disclosure, disruption, modification, or destruction. They establish a critical baseline for organizations to ensure the security of their data.

**Core Goals — the CIA Triad:** the primary objective of these standards is to maintain the CIA Triad, the foundational model for designing secure systems:
- **Confidentiality** — preventing unauthorized access to or disclosure of sensitive information
- **Integrity** — ensuring that information is accurate, complete, and remains unaltered by unauthorized users
- **Availability** — ensuring that systems, networks, and data are accessible and operational for authorized users whenever needed

**The Importance of INFOSEC:**
- **Data Protection** — safeguards sensitive information like financial records, passwords, and personal data
- **Business Continuity** — helps organizations survive disasters, cyberattacks, or system failures through backup and recovery strategies
- **Trust Building** — security measures foster trust among customers, employees, and partners
- **Crime Prevention** — reduces risks associated with hacking, malware, phishing, ransomware, and insider threats
- **Legal Compliance** — ensures organizations meet legal and regulatory requirements, such as GDPR, HIPAA, and ISO/IEC 27001

**Influential Global Organizations Shaping Internet and Security Standards:**

| Organization | Founded | Based |
|---|---|---|
| International Telecommunication Union (ITU) | 1865 | Geneva, Switzerland |
| Internet Engineering Task Force (IETF) | 1986 | Global community, USA |
| World Wide Web Consortium (W3C) | 1994 | Cambridge, Massachusetts, USA |
| Internet Society (ISOC) | 1992 | Reston, Virginia (USA) and Geneva, Switzerland |
| Internet Corporation for Assigned Names and Numbers (ICANN) | 1998 | Los Angeles, California, USA |
| UNESCO | 1945 | Paris, France |

## 2. Deep Dive into the CIA Triad

The CIA Triad is the foundational model in cybersecurity, guiding the design of all secure systems, policies, and controls:

- **Confidentiality** — ensures sensitive information is accessed only by authorized individuals or systems. Achieved through encryption (e.g. AES-256 for data at rest and TLS/SSL for data in transit), role-based access controls (RBAC), multi-factor authentication (MFA), data masking, and physical security like biometric access to server rooms.
- **Integrity** — guarantees that data is accurate, complete, and has not been altered by unauthorized parties. Maintained using hashing algorithms (like SHA-256) to verify data consistency, digital signatures for authenticity, version control systems, and detailed audit logs.
- **Availability** — ensures that systems, networks, and resources are operational and accessible to authorized users whenever they are needed.

## 3. The NIST Cybersecurity Framework (CSF)

Developed by the National Institute of Standards and Technology (NIST) in the USA, the NIST CSF is a guideline or framework rather than a certification standard. Its primary purpose is to help organizations manage and reduce cybersecurity risk through a clear, rule-based roadmap that removes guesswork from security planning. The framework is flexible enough to work for organizations of any size across all industries and provides a universal vocabulary for technical teams, management, and regulators.

**The NIST SP 800 Series:** NIST also published the SP 800 series, which has become a widely used standard even outside government because it provides clear, practical, trusted guidance — including specific instructions on security controls, risk assessment, incident response (IR), access control, logging, monitoring, and system security planning.

### Core Functions of the NIST CSF

The framework organizes security into five common functions that guide an organization from planning to recovery:

1. **Identify (ID)** — understanding the business context, critical assets, and risks to make correct security decisions. Activities include:
   - **Asset Management** — knowing devices, apps, and data
   - **Business Environment** — identifying critical services (e.g. payment systems)
   - **Governance** — defining policies and compliance requirements
   - Conducting risk assessments to identify threats and vulnerabilities
2. **Protect** — implementing security measures to safeguard critical infrastructure and data, to limit or stop potential attacks
3. **Detect (DE)** — monitoring systems to find cybersecurity events quickly before damage becomes significant. Includes defining "abnormal" behavior (Anomalies and Events) to alert on suspicious activity, and maintaining Security Continuous Monitoring through logs across network and cloud endpoints
4. **Respond (RS)** — when an incident occurs, the organization must act to contain the impact. Requires an incident response plan with defined roles (legal, tech, communications), immediate containment actions (such as rotating keys), forensics to preserve logs, and transparent updates to affected stakeholders
5. **Recover** — restoring normal operations and improving resilience. Includes verifying clean backups, restoring services, and conducting post-incident reviews to understand why a failure occurred (e.g. a misconfiguration), updating change-management processes, and preventing recurrence

### NIST Incident Response Guidelines

When a breach occurs, NIST provides a clear step-by-step process for handling it:
1. **Defining Actions** — establishing an order of operations (containment, investigation, and eradication)
2. **Evidence Handling** — preserving logs and system data for legal or investigative purposes
3. **Structured Recovery** — ensuring services are restored safely
4. **Lessons Learned** — reviewing the incident to prevent future occurrences

### Benefits of Using NIST CSF

- **Risk Management** — helps organizations prioritize security investments based on actual risk
- **Universal Language** — provides a common vocabulary for technical teams, management, and regulators
- **Compliance Support** — while not a law itself, it aligns with regulations like GDPR, HIPAA, and ISO/IEC 27001
- **Flexibility** — designed to work for organizations of any size across all industries

## 4. Case Study: SaaS Cloud Bucket Misconfiguration

This case illustrates the Respond and Recover functions in practice:

- **Immediate Response** — the organization must activate its IR plan, assigning roles to an IR lead, legal, and tech teams. The exposed bucket is immediately made private, and security keys are rotated (Containment). Affected clients are given transparent updates (Communication).
- **Recovery and Forensic Analysis** — the team preserves logs to build a timeline of the breach and restores services only after verifying clean backups.
- **Post-Incident Review** — the organization conducts a review to ask "Why did the misconfiguration happen?" This leads to a new change-management process where every cloud change requires peer review and approval, alongside regular security audits.

---
*Source: coursework notes (AI-compiled study notes), International and Federal INFOSEC Standards module. NIST CSF content originally appeared twice across two source lecture parts at different levels of detail; merged into one section here.*
