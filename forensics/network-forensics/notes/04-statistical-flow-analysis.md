# Statistical Flow Analysis

Statistical Flow Analysis (SFA) is a method used to identify compromised machines, cross-reference Data Leakage Prevention (DLP) findings, and profile network users. Unlike deep packet inspection, which looks at the payload, SFA focuses on the metadata of the communication — and is specifically useful for profiling individuals to determine their work schedules, periods of inactivity, or even sources of entertainment while at work.

## The Flow Record and Processing Systems (FRPS)

- **Flow Record**: metadata about the network flow, including IP addresses, port numbers, date, time, and the amount of data exchanged.
- **FRPS components**:
  - **Sensor**: monitors network traffic and generates flow records.
  - **Collector**: a server that receives and stores records from the sensor.
  - **Aggregator**: sorts and manages data coming from multiple collectors.
  - **Analyzer**: processes data to produce meaningful information and reveal problems.

## NetFlow and IPFIX

- **NetFlow**: developed to manage large amounts of data by removing the packet payload and keeping only header details (e.g., IPs, ports, protocol, flags, time). It can be thought of as a "phone bill" for network traffic. Versions v1 through v10 exist, with v5 and v10 (IPFIX) the standards in common use.
- **IPFIX**: also known as NetFlow v10, a widely used protocol for exporting flow information.
- **Flow directions**:
  - **Uniflow**: views traffic as two separate entities (send vs. receive).
  - **Bitflow**: views traffic as a single bidirectional entity.

## Sensor Deployment Types

The visibility of network traffic depends on where sensors are placed:

- **Perimeter Visibility**: sensor placed between the firewall and the internal router.
- **Enclave Visibility**: sensors placed on switches to monitor specific network segments.
- **Host-Flow Visibility**: the sensor is placed directly on the endpoint device.

## Essential Tools and Utilities

- **YAF (Yet Another Flowmeter)**: processes PCAP files or live traffic into bidirectional flows in the IPFIX format.
- **SiLK (System for Internet-Level Knowledge)**: a suite used for collecting, storing, and analyzing large network datasets.

**Key SiLK commands**

| Command | Purpose |
|---|---|
| `rwfileinfo` | Prints metadata about a SiLK flow file (e.g., record counts, version). Use `--field` with a numeric prefix (e.g., `7`) to print specific metadata like the record count. |
| `rwcut` | Displays records as readable text. Default delimiter is `\|`, customizable with `--column-sep`. |
| `rwtotal` | Summarizes flow records by a specified key, such as destination port. Can summarize by subnet using `--sip-first-16` or `--sip-first-24`. |
| `rwuniq` | Summarizes records based on a specific field. |
| `rwstats` | Displays top or bottom results (e.g., top 10 ports by packet count). |
| `rwcount` | Breaks records into specific time intervals to identify traffic spikes. |
| `rwfilter` | Often called the "Swiss Army knife" for filtering specific flows (e.g., traffic from port 80). |
| `rwscan` | Detects scanning activities, such as port scans. |

**Wireshark**: provides basic flow features like Protocol Hierarchy (list of protocols/bytes), I/O Graphs (traffic spikes), Packet Counters, and an HTTP Packet Counter that categorizes responses into 4xx (client error) and 5xx (server error) for quickly identifying application errors.
