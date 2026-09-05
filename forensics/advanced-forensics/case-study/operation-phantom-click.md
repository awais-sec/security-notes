# Case Study: Operation Phantom Click

*Course-provided case study prompt for Advanced Computer Forensics, transcribed from a printed handout. This is the scenario as given; no answers are included here.*

**Level:** Moderate
**Course:** Advanced Computer Forensics
**Audience:** BS Computer Forensics and Cyber Security students

## Scenario Overview

FinSecure, a mid-sized fintech company specializing in digital financial services, recently launched a beta analytics dashboard for VIP clients. The feature was deployed on an internal Linux-based API server: `api-vip.finsecure.net`. Less than a week after deployment, FinSecure's SOC (Security Operations Center) observed:

- Anomalous DNS resolutions to an external domain, `trackmyexpenses.xyz`
- Outbound HTTPS traffic from the API server at irregular intervals
- Customer complaints about unauthorized transactions

The SOC team initiated a full incident response. The following digital artifacts were preserved for forensic analysis:

1. System logs (`/var/log/syslog`) from `api-vip.finsecure.net`
2. Full memory dump of the API server
3. PCAP (packet capture) covering April 2–6
4. Internal Git commit logs
5. Corporate DNS and firewall logs
6. VPN login records
7. Browser history and cache from the DevOps lead's workstation
8. The content of a cron job script named `analytics_update.sh`

## Key Timeline Events

- **March 29**: DevOps lead is assigned to enhance the VIP analytics feature.
- **April 1**: DevOps lead imports code from an external GitHub repo, `vip-enhancer`.
- **April 2 (11:35 PM)**: Cron job `analytics_update.sh` is scheduled to run every 15 minutes.
- **April 3**: DNS logs show first resolution of `trackmyexpenses.xyz`.
- **April 5**: Outbound data spikes; three VIP clients report fraudulent transactions.
- **April 6**: Malware `PhantomClicker` discovered as a hidden component inside the analytics cron script.

## Detailed Forensic Clues

**System Logs**

- `sudo` activity logged for the DevOps lead.
- Installation of the cron job aligns with their login session.

**analytics_update.sh (Extracted)**

- Contains a base64-encoded payload.
- On decoding, shows a Python script initiating outbound socket connections.
- Script masks traffic using HTTPS and pretends to fetch JSON from `trackmyexpenses.xyz/updates`.

**Memory Dump**

- Reveals a running Python process, `/usr/bin/.hiddenpy`.
- Process memory includes decrypted credentials and session tokens.

**PCAP Analysis**

- Shows HTTPS connections to a single IP (associated with `trackmyexpenses.xyz`) every 15 minutes.
- Payload size increases during VIP login windows.

**GitHub Repository Review**

- Imported without internal security audit.
- Contains commits by a pseudonymous user, `codefox2025`.
- Uses obfuscated code: excessive use of `lambda`, `eval`, and base64 libraries.

**DNS and Firewall Logs**

- Persistent DNS resolutions to `trackmyexpenses.xyz` post-deployment.
- Data exfiltration through POST requests to port 443.

**VPN Records**

- Only the DevOps lead had active remote sessions on April 1 and 2.
- Sessions overlap with the introduction of the cron job and the external Git pull.

**Browser Cache from DevOps Lead Workstation**

- Access to pastebin.com and temp-mail.org.
- Search history includes "how to disguise python malware," "crontab backdoor," and "github fake contributor."

## Your Task

Based on the forensic data provided, answer the following questions:

1. What method was likely used to deliver the malware?
2. What are the key IOCs in the analytics script?
3. What was the Python process doing in memory?
4. How do DNS and firewall logs help confirm data exfiltration?
5. What in the packet capture indicates command-and-control behavior?
6. What does the Git commit timeline tell us?
7. What was suspicious in the GitHub repository?
8. Who had remote access at the time of the attack?
9. What does browser cache reveal about intent?
10. Who is the likely perpetrator, and why?
