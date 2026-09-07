# Investigation Methodology and Evidence Sources

## Why Perform Network Forensics?

- **Scalability**: in a large corporate infrastructure with thousands of systems, it's impossible to image and analyze every single one.
- **Problematic scenarios**: disk drives may not be available for analysis, or the attack may be in progress and investigators may not want to alert the attackers.
- **Artifacts of intrusion**: when a crime happens over a network ("the wire"), it leaves behind artifacts. These help investigators understand the intent of the attack, the actions performed by the attackers, and what happened next if the attack was successful.
- **Severe attacks**: Advanced Persistent Threats (APTs), ransomware, and espionage often start with a single unauthorized entry and evolve into long-term campaigns. Information during this period flows through many devices (routers, firewalls, proxies, etc.), creating a trail of evidence.

## Investigation Methodology: The OSCAR Framework

A structured approach to ensure consistent results: **O.S.C.A.R.**

**1. Obtain Information**

- Goal: gather initial information about the incident and the environment.
- Details: familiarizes the investigator with the incident type, timelines, and the people, systems, and endpoints involved.

**2. Strategize**

- Goal: develop a plan, as logs from various devices differ in nature and volatility (e.g., firewall logs vs. ARP tables).
- Strategic points: define clear goals and timelines, find the sources of evidence, analyze the cost and value of the evidence sources, prioritize the acquisition of evidence, and plan timely updates for the client.

**3. Collect**

- Goal: acquire evidence according to the strategic plan.
- Actions: document all accessed systems, capture data streams, and collect logs from servers and firewalls.
- Best practices: make copies of evidence and generate cryptographic hashes (for verification); never work on the original evidence, use copies; use industry-standard tools; document all actions.

**4. Analyze**

- Goal: the core phase where data is examined to solve the puzzle.
- Methods: use automated and manual techniques with various tools to correlate data from different sources, establish a timeline of events, eliminate false positives, and create working theories supported by evidence.

**5. Report**

- Goal: present findings in a clear, understandable manner for non-technical audiences (legal teams, lawyers, juries, insurance).
- Content: the report should contain executive summaries backed by technical evidence, and effectively explain the entire investigation.

## Sources of Network Evidence

**1. Tapping the Wire and the Air**

- Wired: using network taps or SPAN ports on switches to snoop and forward all traffic to an analyzer.
- Wireless (Wi-Fi): using a wireless adapter in promiscuous mode to capture all traffic for a specific access point and channel.

**2. CAM Table on a Network Switch**

- Stores the mapping between a device's MAC address and the physical switch port.
- Helps pinpoint the physical location of a device on the network.
- Switches can provide network mirroring to see data from other VLANs.

**3. Routing Tables on Routers**

- Maps router ports to the networks they connect.
- Helps investigate the path that network traffic takes.
- Routers often have built-in packet filters and firewalls that can log denied or specific types of traffic.

**4. DHCP Logs**

- Logs when an IP address is assigned to a MAC address, lease renewals, and timestamps.
- Provides a list of all dynamically allocated hosts on the network (e.g., a DHCP Clients Table showing hostnames, IPs, and MAC addresses).

**5. DNS Server Logs**

- Records all domain name resolution queries.
- Crucial for identifying Indicators of Compromise (IoCs). For example, if an infected system queries a known malicious domain (e.g., `malware.samples.com`), the log reveals the internal IP making the request.

**6. Domain Controller / Authentication Servers / System Logs**

- Records login attempts, times, and other authentication-related activities.
- Can reveal compromised systems being used to attack other internal systems (pivoting), showing failed/successful login attempts.

**7. IDS/IPS Logs**

- Among the most helpful logs for forensics.
- Provide details on matched attack signatures, ongoing attacks, malware, command-and-control servers, source/destination IPs and ports, and timelines.

**8. Firewall Logs**

- Provide a detailed view of network activity.
- Show connection attempts, blocked traffic, traffic types, and trust scores for outbound endpoints.

**9. Proxy Server Logs**

- Useful for uncovering internal threats.
- Provide explicit details on user web surfing habits, sources of web-based malware, and user behavior on the network.

## Technical Concepts: The OSI Model Refresher

A 7-layer model for network communication:

| Layer | Name | Purpose & Examples | Data Unit |
|---|---|---|---|
| 7 | Application | End-user protocols (HTTP, DHCP, FTP, SSH) | Data |
| 6 | Presentation | Translation, encryption, compression (SSL/TLS, Kerberos) | Data |
| 5 | Session | Establishment, maintenance, termination of sessions (RTP, SOCKS) | Data |
| 4 | Transport | Host-to-host communication, segmentation (TCP, UDP) | Segment |
| 3 | Network | Logical addressing and routing (IP, ICMP, OSPF) | Packet |
| 2 | Data Link | Node-to-node data transfer (Ethernet, Wi-Fi, PPP) | Frame |
| 1 | Physical | Physical cabling, electrical signals (cables, hubs) | Bit |

**Data encapsulation flow (how browsing a website works)**

1. Application (Layer 7): you type a URL. The domain name is resolved to an IP address.
2. Transport (Layer 4): data is encapsulated with a TCP/UDP header (adds source/destination ports).
3. Network (Layer 3): data is encapsulated with an IP header (adds source/destination IP addresses).
4. Data Link (Layer 2): the entire packet is framed with an Ethernet header (adds MAC addresses).
5. Physical (Layer 1): the frame is converted into bits and sent over the wire.
6. On the receiving end: the process reverses, with each layer stripping off its respective header until the payload is delivered to the application.

**OSI vs. TCP/IP model**: the mapping isn't perfect. The TCP/IP model is a condensed 4-layer model — its Application layer encompasses OSI layers 5–7, its Transport layer aligns with OSI layer 4, its Internet layer aligns with OSI layer 3, and its Network Access layer encompasses OSI layers 1–2.

**Key takeaway**: as information travels, it creates traces (logs) on various network devices, which become crucial sources of evidence.

## Log-Based Evidence

**Why logs are crucial**: when packet capture files are not available, investigators must rely solely on logs from endpoints (servers, databases, firewalls) to deduce what happened.

### Scenario: Acme Inc. Data Breach

- **Attack path**: Attacker → External Application Server → Internal Database.
- **Key investigative questions**: how was the application server penetrated? Why did the firewall allow the attacker? What queries did the attacker execute on the database? Was the database altered? What is the origin of the attack?
- **Required logs**: application server logs, firewall logs, and database logs.

**A. Application Server Logs (e.g., Apache)**

- Location: typically `/var/log/apache2/access.log` and `error.log`.
- Access log format: `IP Address - - [Date/Time] "Request Method Requested_Resource HTTP_Version" Response_Code Response_Length "User-Agent"`.
- What it reveals: source IP of the attacker; requested resources (e.g., `/@eye.php`, `1941.php`); tools used (the User-Agent string can reveal scanning tools like DirBuster).
- Error logs show errors like "permission denied," resulting in 403 Forbidden status codes, revealing the attacker's attempts to access non-existent or restricted files.
- Analysis tool: manual analysis is difficult; use automated tools like Apache Logs Viewer to filter, sort, and analyze log data efficiently.

**B. Database Logs (e.g., MySQL)**

- General Query Log: contains a record of all queries executed on the database.
- What it reveals: brute-force attacks (multiple "Access denied" entries for the root user from a single IP); reconnaissance (queries like `show tables;`, `show variables;`, `select user, host, password from mysql.user;` indicate the attacker is mapping the database structure and looking for user credentials).
- Enabling logs in MySQL:

  ```sql
  SET global general_log = 1;
  SET global general_log_file='/tmp/mysql.log';
  SET global log_output = 'file';
  SET global general_log = on;
  ```

**C. Firewall Logs**

- Challenge: firewall logs are complex and difficult to parse manually.
- Solution: use specialized log parsers and analytics engines.
- Example tool: Sawmill — a third-party log parser with a free 30-day trial, able to parse Firewall, Proxy, and IDS/IPS logs, and provide summaries by user, host, source IP, visited pages, bytes transferred, and session duration.

**D. Other Logs**: the same tool (Sawmill) can also be used to parse and analyze Proxy Logs and IDS/IPS Logs, providing a centralized view of network activity.
