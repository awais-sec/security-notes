# Investigating Web Attacks

## Objectives

- Understand web application forensics.
- Understand Internet Information Services (IIS) logs.
- Understand Apache web server logs.
- Overview of web attacks on Windows-based servers.
- Understand how to detect and investigate various attacks on web applications.

## Introduction to Web Application Forensics

- **Definition**: Web application forensics involves tracing a security attack on a web application to identify its origin and method of penetration.
- **Process**: Involves collecting and analyzing log and configuration files from the web server, application server, database server, and system events.
- **Purpose**: Determine the cause, nature, and perpetrator of a web exploit.

## Challenges in Web Application Forensics

- **Distributed nature**: Activities are recorded across numerous hardware and software components.
- **Limited downtime**: Investigations must be conducted with minimal or no downtime.
- **Volume of logs**: Requires analysis and correlation of large volumes of logs.
- **Knowledge requirement**: Needs comprehensive understanding of web servers, application servers, databases, and underlying applications.
- **Tracing difficulty**: Challenging when reverse proxies or anonymizers are used.

## Indicators of a Web Attack

- Customers unable to access services
- Web page defacements
- Suspicious activities in user accounts
- Leakage of sensitive data
- URLs redirecting to incorrect sites
- Unusually slow network performance
- Frequent server rebooting

## Web Application Threats

| # | Threat |
|---|---|
| 01 | Cookie Poisoning |
| 02 | SQL Injection |
| 03 | Injection Flaws |
| 04 | Cross-Site Request Forgery |
| 05 | Directory Traversal |
| 06 | Unvalidated Input |
| 08 | Sensitive Data Exposure |
| 10 | Denial of Service (DoS) |
| 11 | Broken Access Control |
| 12 | Security Misconfiguration |
| 14 | Improper Error Handling |
| 15 | Buffer Overflow |
| 17 | Broken Authentication |
| 18 | Log Tampering |

## Web Attack Investigation Methodology

1. **Conduct interviews**: Gather information from individuals involved.
2. **Locate and secure devices**: Identify servers/devices involved, take them offline, and seize them in a forensically sound manner.
3. **Forensic image acquisition**: Create and duplicate forensic images.
4. **Collect logs** from:
   - Web server
   - Application server
   - Database server
   - Web application firewall
   - Local system events
   - SIEM tool
   - Intrusion Detection System (IDS)
   - Application and server configuration files
5. **Protect log integrity**: Use encryption and checksums to verify and secure log files.
6. **Analyze logs**: Examine working copies of logs for suspicious entries and correlate data to reconstruct the attack scenario.
7. **Trace the attacker**: Identify the perpetrator by tracing the attacking IP.
8. **Document the investigation**: Record every step of the investigation process.

## IIS Web Server Architecture

Covers the architecture of Internet Information Services (IIS) web servers at a high level (specific architectural detail not included in the source material).

## IIS Logs

- **Function**: IIS logs record all server visits in ASCII text-based log files.
- **Information captured**: client IP address, username, date and time, request type, target of operation.
- **Default storage location**: `SystemDrive\inetpub\logs\LogFiles` on Windows Server OS.
- **Viewing**: logs can be viewed in a text editor, showing entries with the fields above.

## Investigating a SQL Injection Attack: Using Regex

- **Approach**: use regular expressions to search log files for SQL-specific meta-characters indicating SQL injection attacks.
- **Meta-characters to detect**: single-quote (`'`), double-dash (`--`), equals sign (`=`), semi-colon (`;`), and their hex equivalents.
- **Regex for MSSQL Server**:

```
/exec(\s|\+)+(s|x)p\w+/ix
```

## Examining IIS Logs for a SQL Injection Attack

- **Encoded query**: `id=ORD$001%27%20or%201=1;--`
- **Decoded query**: `id=ORD-001 ' or 1=1;--`
- **Indication**: SQL injection attack bypassing authentication.

**Details extracted:**

- **Date**: 13 December 2019
- **Attacker IP**: 10.10.10.55 (Linux machine)
- **Server IP**: 10.10.10.12
- **Website**: www.luxurytreats.com
- **Username**: bob
- **Target**: order details page
- **Malicious query**: `' or 1=1;--`
- **HTTP status**: 200 (request processed, allowing unauthorized data access)

## Examining Snort Alert Logs for a SQL Injection Attack

- **Attacker IP**: 192.168.0.233
- **Source port**: 64580
- **Server IP**: 192.168.0.115
- **Destination port**: 80
- **Purpose**: Snort alerts indicate an attempted SQL injection attack.

## Examining SIEM Logs for a SQL Injection Attack

SIEM tools aggregate and analyze logs, generating alerts for SQL injection attempts (specific SIEM output detail not included in the source material).
