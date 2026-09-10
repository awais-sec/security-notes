# Incident Response and Management: Fundamentals and the Ransomware Threat Landscape

## 1. Course Overview & the Role of Incident Response

**Definition:** Incident Response and Management focuses on how organizations prepare for, detect, respond to, and recover from security incidents.

**Scope of Incidents:** cyber-attacks, data breaches, malware infections, ransomware, insider misuse, and system outages.

**The IR Role:** incident response provides a specialized, systematic, repeatable, and consistent service for handling incidents.

**Core Objectives:** prevent, detect, and respond to threats while maintaining compliance with policy and law.

**Types of Incidents Managed:** phishing and worms; vulnerabilities and misconfigurations; insider threats and hacked accounts; denial of service (DoS) and brute force attacks; property/data loss and ransomware.

## 2. Historical Foundations of Incident Response

**Pre-1988 — the "Fix It When It Breaks" Era:** early networks were small, and incidents were handled informally by system administrators without standard teams or official protocols.

**The Morris Worm (November 1988):**
- **Significance** — one of the first worms to spread widely, overwhelming UNIX systems and proving that the internet required organized, coordinated responses rather than ad-hoc fixes
- **Infection Path** — exploited multiple paths, including a vulnerability in the sendmail service
- **Impact** — a key mistake in its design caused it to re-infect machines repeatedly, effectively creating a denial-of-service (DoS) effect that crashed systems
- **Legacy** — DARPA tasked the Software Engineering Institute (SEI) at Carnegie Mellon to form the CERT Coordination Center (CERT/CC), establishing the first major organized incident response capability

**Global Collaboration (1990):** the Forum of Incident Response and Security Teams (FIRST) was founded to connect response teams worldwide for information sharing, maturing IR into a community-based defense.

**Formalization (2000s–2010s):** organizations began adopting repeatable procedures and formal guides, such as NIST SP 800-61, which established standard phases for incident handling.

**Modern Integration (2020s):** incident response has merged with business risk management, involving legal, PR, and compliance departments to address modern threats like cloud security and ransomware.

## 3. Understanding Ransomware

**Definition:** malware that prevents or limits users from accessing their system, often by encrypting data in an unrecoverable fashion, and demands payment for the decryption key.

**The Ransom Mechanism:** victims are forced to use specific online payment methods (often cryptocurrency) to regain access to their data.

**Human-Operated Ransomware:** unlike "worms" that spread automatically, human-operated attacks involve real attackers manually navigating a network for days or weeks to steal data and disable defenses before the final encryption strike.

## 4. Evolution of Human-Operated Ransomware

The threat landscape shifted from uncontrolled outbreaks (like WannaCry) to targeted, manual "Big Game Hunting" against entire enterprises.

**2016 — SamSam Era:** operators focused on lateral movement and encrypting as many devices as possible rather than targeting single users, marking a drastic change in the threat landscape. Notable targets included healthcare, education, and cities.

**2017 — BitPaymer & "Big Game Hunting":** associated with the group "Evil Corp," this strain introduced the trend of Big Game Hunting — targeting large organizations for astronomical ransoms.

**2018 — Ryuk:** operated by "Wizard Spider," used the "Triple Threat" chain (Emotet → Trickbot → Ryuk); earned an estimated $150 million; more recently used BazarLoader and vishing.

**2019–Present — Ransomware-as-a-Service (RaaS):** the current dominant trend, where developers lease ransomware to affiliates for a percentage of the profit. This model allows unskilled cybercriminals to participate in high-level attacks, contributing to what's described as a "cyberpandemic."

*(See [[incident-response-ransomware-case-studies]] for detailed case studies on SamSam, BitPaymer, Ryuk, Egregor, and Hive.)*

## 5. The RaaS Economy and Profit Sharing

Modern ransomware operates as a professional business with a clear division of labor and profits:

- **Ransomware Developers** — create the software and run the program (receive ~20% of the ransom)
- **Affiliates** — execute the actual attack (receive ~50% of the ransom)
- **Initial Access Brokers** — provide entry into compromised networks (receive ~10% of the ransom)
- **Specialists** — the remaining funds go to hired pentesters (for privilege escalation) or professional negotiators

**Market Share:** RaaS affiliates performed 64% of all 2020 attacks.

**Division of Labor:** affiliates often do not perform the entire attack themselves. They may hire Initial Access Brokers to get into a network or hire specialized "pentesters" for privilege escalation and defense evasion.

**Double Extortion:** modern attackers steal sensitive data before encrypting it. If the victim refuses to pay for decryption, the attackers threaten to leak the data on a Dedicated Leak Site (DLS) to increase pressure. This technique was pioneered by affiliates of the Maze ransomware group in 2019.

## 6. The General Incident Response (IR) Life Cycle

The IR process is a systematic, repeatable, and consistent cycle designed to handle security incidents efficiently, standardized under NIST SP 800-61 (Computer Security Incident Handling Guide).

1. **Preparation** — the goal is to be ready before an incident occurs. Activities include creating policies and procedures, defining team roles, deploying security tools (SIEM, antivirus, backups), and conducting staff training or mock drills (e.g. phishing recognition). Default logging is usually insufficient; enable advanced logging before an attack happens, and retain logs for weeks/months since attackers often enter long before deploying ransomware. The IR team can be internal (SOC), a third-party vendor, or Managed Detection and Response (MDR). XDR allows for active blocking/isolation; SIEM is best for long-term log storage.
2. **Identification (Detection & Analysis)** — detecting and confirming an incident has occurred. Activities include monitoring security alerts, analyzing logs, and determining the scope and severity of the threat (e.g. detecting multiple failed logins followed by a successful one from an unusual location). Identify the ransomware strain via the ransom note or communication portal. Use Cyber Threat Intelligence platforms to find specific TTPs and IoCs. Monitor for "loader" trojans (Trickbot, BazarLoader) or recon tools (AdFind) to stop attacks early.
3. **Containment** — stopping the incident from spreading to limit further damage. Block Command & Control (C2) IPs; isolate compromised hosts; block cloud storage (MEGA) and remote tools (AnyDesk, TeamViewer); apply temporary fixes.
4. **Eradication** — completely removing the cause of the incident. Change all compromised passwords; remove malware and persistence (registry keys, services, scheduled tasks); close vulnerabilities and apply security patches.
5. **Recovery** — safely restoring systems and services to normal operation. Reimage hosts if cleanliness isn't 100% certain (be cautious of buggy decryptors — e.g. ProLock — that can corrupt large files); restore from clean backups; test system security and monitor for suspicious activity post-restoration.
6. **Lessons Learned (Post-Incident Review)** — improving future responses based on the current experience. Document response actions and perform root cause analysis. Harden systems (patch vulnerabilities, implement MFA for all RDP/VPN connections), conduct spear-phishing awareness training and use email sandboxes, and secure backups following the 3-2-1 rule (3 copies, 2 media types, 1 offsite) with separate accounts for backup servers.

## 7. The Life Cycle of a Human-Operated Ransomware Attack

Unlike opportunistic ransomware that spreads automatically (like a worm), human-operated attacks are manual, targeted, and carefully planned for maximum impact.

| Stage | Description |
|---|---|
| A — Initial Access | See section 8 below for detailed vectors |
| B — Foothold & Persistence | Attackers enter the organization and ensure they can return even if one access method is removed, using backdoors, remote access tools, and scheduled tasks or abused accounts |
| C — Privilege Escalation | The goal is to move from normal user access to administrator or domain admin level, via credential dumping (stealing passwords/hashes) and abusing misconfigurations |
| D — Reconnaissance | Attackers map the environment, identifying Active Directory structures, key servers (email, database, backups), security tools like EDR/SIEM, and "crown jewels" (valuable data) |
| E — Lateral Movement | Using stolen credentials, attackers spread inside the network from one machine to another via remote admin tools or shared folders |
| F — Data Discovery & Exfiltration | Modern attackers often steal sensitive data before encryption ("double extortion") for extra pressure |
| G — Defense Evasion & Backup Disruption | Attackers try to block recovery by disabling security monitoring, deleting logs, and damaging or deleting backups |
| H — Ransomware Deployment (Mass Encryption) | The "final strike" where endpoints and servers are encrypted, file extensions are changed, and ransom notes are dropped |
| I — Extortion & Negotiation | Attackers set deadlines and negotiate via chat portals; the victim organization must decide between restoring from backups or negotiating while managing legal and regulatory reviews |
| J — Post-Incident Recovery | Even after restoration, risks remain, such as potential data leaks of already exfiltrated files or hidden backdoors that could lead to repeat attacks if root causes aren't fixed |

## 8. Detailed Initial Access Vectors (Stage A)

The top 3 initial attack vectors are RDP compromise, spear phishing, and software vulnerabilities.

- **RDP (Remote Desktop Protocol)** — historically the most common access method; millions of servers are exposed via port 3389, allowing attackers to control a computer remotely as if they were sitting in front of it
- **Access Brokers** — specialized actors who sell network access to ransomware affiliates for approximately 10% of the ransom
- **Spear Phishing & Vishing** — uses "thread hijacking" (replying to real emails) or "vishing" (voice calls) to deliver trojans like BazarLoader, Qakbot, or Trickbot. Groups like Ryuk have used vishing, calling victims and guiding them to download weaponized Office files and enable macros.
- **VPN Vulnerabilities** — focus on Remote Code Execution (RCE) in VPNs/Gateways (e.g. Pulse Secure, FortiGate, Citrix); attackers exploit security bugs or outdated software to access a company network without a password
- **Zero-Days** — rare but high-impact (e.g. REvil's $70M Kaseya attack)
- **Initial Access Brokers** — professional actors who specialize in gaining entry to networks and then selling that access to ransomware operators

## 9. Cyber Threat Intelligence (CTI)

CTI is a vital component of incident response, divided into three primary levels answering the who, why, how, where, and what of an attack:

- **Strategic Intelligence (Who & Why)** — for decision-makers (CISO, CIO, CTO); focuses on high-level trends, financial motives, and target sectors (e.g. Hive targets healthcare)
- **Operational Intelligence (How & Where)** — for SOC analysts, incident responders, and threat hunters; focuses on Tactics, Techniques, and Procedures (TTPs) using the MITRE ATT&CK framework:
  - Tactics — goals (e.g. Initial Access)
  - Techniques — general means (e.g. Spear Phishing)
  - Sub-techniques — specific means (e.g. weaponized attachment)
  - Procedures — exact implementation (e.g. a specific malicious DOCM file)
- **Tactical Intelligence (What)** — for security products (SIEM/firewalls); focuses on Indicators of Compromise (IoCs) like IP addresses, domains, and file hashes (MD5, SHA1, SHA256). These often have a short lifecycle since attackers frequently change their tools and infrastructure.

**Sources of Intelligence:**
1. Threat Research Reports — detailed technical analysis from security vendors
2. Community — real-time TTP sharing on Twitter and LinkedIn by researchers (e.g. identifying ProxyLogon as an access vector for RagnarLocker)
3. Threat Actors — monitoring underground forums where actors buy exploits (e.g. SonicWall VPN) or recruit RaaS affiliates

**Analysis Tools:** VirusTotal for analyzing malicious content, and MISP (Malware Information Sharing Platform) for sharing indicators across the security community.

## 10. MITRE ATT&CK Mapping (Common Examples)

| Tactic | Technique / Sub-technique |
|---|---|
| Execution | Windows Command Shell (T1059.003), PowerShell (T1059.001) |
| Defense Evasion | Rundll32 (T1218.011), Software Packing (T1027.002), File Deletion (T1070.004) |
| Credential Access | LSASS memory (T1003.001), Registry modification to cache cleartext credentials (T1112) |
| Impact | Service stop (T1489), Inhibit system recovery (T1490), Data encrypted for impact (T1486) |

Investigators build an attack timeline by looking at evidence from filesystems, registry, memory, and logs (Windows Event logs, PowerShell logs, EDR alerts).

## 11. The Pyramid of Pain

This model describes how difficult it is for an attacker to change their behavior when a defender detects a specific indicator, ranked from easiest to hardest for an attacker to change:

1. **Hash Values (Trivial)** — very easy for attackers to change by modifying a single bit of a file
2. **IP Addresses (Easy)** — attackers can easily switch to new proxy servers or VPNs
3. **Domain Names (Simple)** — registering a new domain is inexpensive and fast
4. **Network/Host Artifacts (Annoying)** — harder to change, as they involve the attacker's tools leaving traces on a system
5. **Tools (Challenging)** — if a specific tool (like Cobalt Strike) is blocked, the attacker must learn or develop a new one
6. **TTPs (Tough!)** — Tactics, Techniques, and Procedures are the "heart" of how an actor operates; changing these requires a complete change in their workflow and training

## 12. Law Enforcement Takedowns

International cooperation has successfully disrupted major threat infrastructures:
- **Emotet Botnet** — in January 2021, authorities from eight countries (including the US, UK, and Ukraine) took full control of the botnet's infrastructure and arrested key operators
- **Cl0p Ransomware** — six members of the group, involved in data laundering, were arrested in Ukraine in June 2021

## 13. Quick Prevention Checklist

Organizations must move beyond just "backups" to a holistic defense:

1. **Identity Security** — use Multi-Factor Authentication (MFA) for all email, VPN, and administrative accounts
2. **Vulnerability Management** — patch all internet-facing systems immediately to prevent initial entry
3. **Network Segmentation** — ensure that users, servers, and backups are separated so that an infection in one area cannot easily spread to the others
4. **Secure Backups** — follow the 3-2-1 rule (3 copies, 2 media types, 1 offline/offsite) and ensure backups are immutable or offline so they cannot be deleted by attackers
5. **Monitoring** — deploy Endpoint Detection and Response (EDR) tools and maintain central logging to catch unusual behavior early
6. **Response Planning** — have a formal incident response plan ready, as the first 60 minutes of an attack are critical for containment

---
*Source: coursework notes (AI-compiled study notes), Incident Response and Management module — merged and deduplicated from two overlapping source documents covering the same fundamentals.*
