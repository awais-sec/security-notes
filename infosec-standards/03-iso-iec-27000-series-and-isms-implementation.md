# The ISO/IEC 27000 Series and ISMS Implementation

## 1. Foundations of the ISO/IEC 27000 Series

The "ISO/IEC" prefix indicates that these standards are jointly published by the International Organization for Standardization (ISO) and the International Electrotechnical Commission (IEC).

- **ISO/IEC 27000** — primarily provides an overview of the ISMS concept and defines key terms and vocabulary (e.g. asset, risk, control), helping align an organization on a common language before implementation
- **ISO/IEC 27001** — the most widely used international standard for an Information Security Management System (ISMS); contains the mandatory requirements that auditors use for certification
- **ISO/IEC 27002** — unlike 27001, this is a "Code of Practice" that provides implementation guidance and best practices for security controls, explaining how to implement the controls effectively
- **ISO/IEC 22301** — the international standard for Business Continuity Management (BCM), ensuring an organization can continue operating during and after a disaster or cyberattack

## 2. The ISMS and PDCA Cycle

The ISO/IEC 27000 family is designed to help organizations build and maintain an Information Security Management System (ISMS) — a structured approach where an organization identifies assets, assesses risks, applies security controls, and continuously monitors performance. This framework follows the Plan-Do-Check-Act (PDCA) cycle:

- **Plan** — establishing the context, leadership, and planning (Clauses 4–6)
- **Do** — implementing support and operations (Clauses 7–8)
- **Check** — performance evaluation (Clause 9)
- **Act** — improvement and corrective actions (Clause 10)

### Mandatory Clauses (4–10)

These clauses contain the "must-have" rules that auditors check for certification:

| Clause | Focus |
|---|---|
| 4 — Context | Understanding the organization's scope and security risks |
| 5 — Leadership | Ensuring top management commitment, policies, and defined roles |
| 6 — Planning | Identifying risks and setting security objectives |
| 7 — Support | Providing necessary resources, training, and documentation |
| 8 — Operation | Running security processes in daily work |
| 9 — Performance Evaluation | Monitoring, auditing, and reviewing the system |
| 10 — Improvement | Fixing issues and updating the system continuously |

## 3. Risk Assessment and Management

Organizations must establish a documented risk assessment methodology that consistently applies criteria such as impact (financial, legal, reputational) and likelihood.

- **Risk Register** — the master document recording identified risks (e.g. phishing, ransomware, cloud misconfigurations), their owners, ratings, and treatment plans
- **Risk Treatment Options** — for every risk, an organization must choose to:
  - **Treat** — apply controls
  - **Accept** — formally tolerate
  - **Avoid** — stop the activity
  - **Transfer** — use insurance or contracts

## 4. Statement of Applicability (SoA)

The SoA is a critical master document that lists every control from Annex A. It specifies which controls are Applicable or Not Applicable, providing a clear justification for each choice and referencing the evidence of their implementation.

Organizations do not need to apply all 93 controls — they choose only those relevant to their specific SoA based on their risk assessment.

## 5. The Certification Process

Certification is maintained through a defined cycle of periodic audits:

1. **Stage 1 Audit** — a readiness check where the auditor reviews documentation like the SoA, scope, and risk treatment plan
2. **Stage 2 Audit** — an implementation audit involving staff interviews and sampling evidence (e.g. MFA proof, log reviews, backup tests)
3. **Surveillance Audits** — conducted annually to confirm the ISMS remains compliant
4. **Recertification Audit** — a comprehensive review typically performed every three years to renew the certificate

**Evidence-Based Auditing:** during audits, professionals do not just look for written policies — they require evidence and records of implementation, such as MFA enrollment logs, proof of backup tests, or signed supplier security clauses.

## 6. Annex A Control Themes (ISO/IEC 27001:2022)

The 2022 version of ISO/IEC 27001 organizes its 93 security controls into four primary themes. Organizations select specific controls from this list based on their unique risk assessment results and document their choices in the SoA.

### Organizational Controls (37 Controls)

Focus on the management framework, policies, and procedural rules of the organization:

- **Policies for Information Security** — document and maintain security rules, such as password and backup policies, reviewed annually
- **Roles and Responsibilities** — security duties must be clearly defined (e.g. HR managing onboarding/offboarding while IT creates access)
- **Segregation of Duties** — high-risk processes should not be controlled by a single person (e.g. one developer writes code while another reviews it)
- **Management Responsibilities** — leadership must enforce security, provide resources, and set key performance indicators (KPIs)
- **Contact with Authorities & Special Interest Groups** — processes must be in place to contact regulators during breaches and maintain links with security communities like CERT/CSIRT for threat alerts
- **Threat Intelligence** — collect and analyze threat updates to take proactive actions like blocking phishing domains
- **Information Transfer** — use secure methods (e.g. encryption, SFTP, or DLP) when sharing data
- **Access Control & Identity Management** — access must follow the "need-to-know"/"least privilege" principle; user identities managed across their entire lifecycle (Joiner–Mover–Leaver) through formal approvals
- **Authentication Information** — passwords, keys, and OTPs protected using tools like password vaults and MFA
- **Supplier & Cloud Security** — security clauses included in contracts (e.g. breach notification timelines), and cloud services must have established security rules, such as prohibiting public storage buckets
- **Incident Management** — organizations must prepare response plans, runbooks, and playbooks to triage events and collect forensic evidence
- **Compliance & Intellectual Property** — identify and follow laws (like PECA or data retention rules) and protect intellectual property using licensed software and restricted repositories

### People Controls (8 Controls)

Manage human behavior to ensure users are trained, authorized, and held responsible:

- **Background Checks** — verify trustworthiness before hiring (e.g. checking criminal records)
- **Security Awareness Training** — train employees on security risks, such as the dangers of opening unknown email links
- **Responsibilities and Policies** — give staff clear rules, such as prohibiting the use of personal USB devices on office computers
- **Access Management for Transitions** — immediately block access when an employee leaves or changes departments
- **Remote Work Security** — employees working from home must follow security rules, such as avoiding public Wi-Fi and using secure connections
- **Event Reporting** — employees are required to report suspicious activity or hacking attempts to the IT team immediately

### Physical Controls (14 Controls)

Secure the physical environment and equipment to prevent unauthorized access:

- **Security Perimeters & Entry Controls** — boundary walls, guards, and access cards or biometrics protect sensitive areas like server rooms
- **Monitoring & Environment Protection** — CCTV for regular monitoring; systems protected from disasters like fire or flood using alarms and extinguishers
- **Working in Secure Areas** — sensitive areas may have special rules, such as prohibiting mobile phones
- **Clear Desk and Clear Screen** — confidential info should not be left visible; computers must be locked when not in use
- **Equipment Siting & Off-Premises Security** — critical equipment in safe locations (e.g. air-conditioned server rooms); assets like laptops kept secure when taken home
- **Storage Media & Utilities** — USB drives and hard disks stored in protected locations like lockers; supporting utilities like electricity (UPS/backup generators) and cabling (protected in walls) secured

**Equipment Lifecycle:**
- **Maintenance** — equipment must be checked and repaired regularly (e.g. bi-annual server maintenance) to prevent failure
- **Secure Disposal** — before disposal or reuse, all data must be permanently removed from devices (e.g. wiping a hard disk completely before selling it to ensure no sensitive data is leaked)

### Technological Controls (34 Controls)

These controls represent technical security measures. The specific list of 34 controls is selected based on an organization's risk profile, and they generally include deeper technical safeguards beyond the organizational and physical realms.

## 7. Continuous Improvement

A critical part of any INFOSEC standard (especially ISO 27001) is the requirement to never stop improving. Organizations must perform root cause analysis after every incident or audit finding to verify that a fix actually worked and to prevent it from happening again.

---
*Source: coursework notes (AI-compiled study notes), International and Federal INFOSEC Standards module.*
