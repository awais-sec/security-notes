# Network Forensics — Notes

Course notes on network forensics, cleaned up and reorganized into markdown.

## Contents

| # | File | Covers |
|---|---|---|
| 01 | [introduction-to-network-forensics.md](01-introduction-to-network-forensics.md) | Why network forensics matters, postmortem vs. real-time analysis, common network and wireless attacks, Indicators of Compromise, the TCP/IP and OSI layers, and types of network-based evidence |
| 02 | [investigation-methodology-and-evidence-sources.md](02-investigation-methodology-and-evidence-sources.md) | The OSCAR investigation framework, sources of network evidence (switches, routers, DHCP, DNS, IDS/IPS, firewalls, proxies), an OSI model refresher, and a worked log-correlation scenario |
| 03 | [deep-packet-inspection.md](03-deep-packet-inspection.md) | What DPI is and why it matters, protocol encapsulation, IPv4/TCP/HTTP header breakdowns, and a practical Wireshark walkthrough for decoding non-standard-port protocols |
| 04 | [statistical-flow-analysis.md](04-statistical-flow-analysis.md) | Flow records, NetFlow/IPFIX, sensor deployment types, and SiLK/Wireshark flow-analysis tools |
| 05 | [tunneling-and-encryption.md](05-tunneling-and-encryption.md) | Decrypting TLS via browser key logs, decoding DNS tunneling with Scapy, WEP/WPA2 wireless decryption, and reconstructing USB keystroke captures |
| 06 | [investigating-malware.md](06-investigating-malware.md) | Malware categories, typical network-based malware lifecycle, and automated sandbox analysis tools |
| 07 | [investigating-c2-servers.md](07-investigating-c2-servers.md) | Command-and-control infrastructure, DGA and fast-flux evasion techniques, and identifying beaconing/C2 traffic patterns |
| 08 | [log-analysis.md](08-log-analysis.md) | DHCP, proxy, and firewall log sources, and a worked example correlating DHCP and proxy logs to reconstruct browsing activity |
| 09 | [wlan-forensics.md](09-wlan-forensics.md) | Monitor mode capture, 802.11 frame types, and decrypting WPA/WPA2 traffic via the four-way handshake |

## Notes

- Files 04–09 are split from a single larger source document covering Chapters 4 through 9; each chapter's supplementary "extras" content has been merged into its relevant section rather than kept separate.
