# Deep Packet Inspection (DPI)

## Introduction to DPI

- **Background**: DPI gained widespread popularity following the Edward Snowden leaks, which revealed extensive government data collection programs.
- **Core concept**: DPI is the process of inspecting the actual data content (payload) of a network packet, going far beyond just analyzing the standard header information (like IP addresses and ports).
- **Capabilities**: devices with DPI can analyze, evaluate, and take action based on information from Layer 2 (Data Link) all the way up to Layer 7 (Application).
- **Key differentiator**: DPI is not reliant solely on port numbers to identify protocols; it looks inside the packet's payload to see what is actually being transmitted.

## Uses and Applications of DPI

1. **Traffic Shaping**: blocking malicious traffic or limiting bandwidth for certain types of data.
2. **Service Assurance**: allowing network administrators to prioritize high-importance traffic (e.g., VoIP) to ensure it is not interrupted.
3. **Identification of Fake Applications**: detecting applications that misuse non-standard ports to disguise their traffic (e.g., FTP running on a port normally used for web traffic).
4. **Malware Detection**: by viewing the payload, DPI can identify malware signatures and communication patterns within the data stream.
5. **Intrusion Detection**: DPI can uncover hack attempts, exploits, backdoor communications, and other malicious activities.
6. **Data Leakage Prevention (DLP)**: DPI can identify and block sensitive or critical data from being exfiltrated from the network.

## Protocol Encapsulation

**What is a network packet?**: in simple terms, it's data packaged to be transferred from one host to another.

**The encapsulation process**: data is wrapped with successive layers of protocol information as it moves down the OSI model before being transmitted.

- **Application Data (Layer 7)**: the actual data (e.g., an HTTP request).
- **TCP/UDP Header (Layer 4)**: adds transport information (e.g., source/destination ports, sequence numbers).
- **IP Header (Layer 3)**: adds network information (e.g., source/destination IP addresses).
- **Ethernet Header (Layer 2)**: adds data link information (e.g., source/destination MAC addresses).

**Summary of roles**: the Ethernet header manages delivery between devices on the same local network; the IP header is responsible for routing the packet from source to destination host across networks; the TCP header manages the reliable communication session between two applications; and the data is the actual application-layer payload.

## Deep Dive into Packet Headers

### A. The IPv4 Header

- **Version**: specifies the IP format (e.g., IPv4).
- **IP Header Length (IHL)**: the length of the IP header itself.
- **Differentiated Services Code Point (DSCP)**: used for prioritizing certain types of traffic (e.g., for real-time communication).
- **Explicit Congestion Notification (ECN)**: allows end-to-end notification of network congestion.
- **Total Length**: the total size of the entire IP packet (header + data).
- **Identification**: a unique identifier for the packet; all fragments of a split packet share the same ID.
- **Flags**: controls whether a router is allowed to fragment (split) the packet.
- **Fragmentation Offset**: indicates the position of a fragment within the original packet.
- **Time To Live (TTL)**: a counter that decreases with each router hop; the packet is discarded if TTL reaches zero, preventing infinite loops.
- **Protocol**: identifies the transport protocol encapsulated in the data (e.g., TCP=6, UDP=17).
- **Header Checksum**: used for error-checking the IP header.
- **Source Address**: IP address of the sender.
- **Destination Address**: IP address of the intended recipient.
- **Options & Padding**: optional fields and extra bits to ensure the header is a standard length.

### B. The TCP Header

- **Source Port**: the port number on the sending host.
- **Destination Port**: the port number on the receiving host.
- **Sequence Number**: the position of the first data byte in the segment.
- **Acknowledgment Number**: the next sequence number the receiver expects, acknowledging receipt of data.
- **Header Length**: the length of the TCP header.
- **Flags (control bits)**: control the connection state — URG (urgent data), ACK (acknowledges received data), PSH (push data to the application immediately), RST (reset/abort the connection), SYN (initiate a connection), FIN (gracefully close a connection), ECE & CWR (used for ECN).
- **Window Size**: the amount of data the receiver is willing to accept (flow control).
- **Checksum**: error-checking for the header and data.
- **Urgent Pointer**: points to the end of urgent data if the URG flag is set.
- **Options & Padding**: additional options and padding for alignment.

### C. The HTTP Packet (Application Layer Example)

An HTTP packet, carried as the TCP payload, typically contains:

- **Request Line**: the method (e.g., `GET`, `POST`), the requested resource (e.g., `/cloudquery.php`), and the HTTP version (e.g., `HTTP/1.1`).
- **Request Message Headers**: `Host`, `User-Agent`, `Content-Type`, etc.
- **Message Body**: the data being sent to the server, such as form parameters or file uploads (e.g., a POST request sending file data to `cloudquery.php`).

## Practical DPI Analysis in Wireshark

**The problem**: protocols can run on non-standard ports (e.g., an FTP server on port 10008). Traditional tools that rely only on port numbers will misidentify this traffic.

**The DPI solution**: DPI looks at the payload to correctly identify the application protocol, regardless of the port used.

**Scenario**: an attacker uses port 443 (typically for HTTPS) for FTP communication to evade detection.

### Steps to Decode a Non-Standard Protocol in Wireshark

1. **Initial capture**: the packet list may not correctly identify the protocol, showing only TCP even if the data contains FTP commands.
2. **Follow TCP Stream**: right-clicking a packet and selecting "Follow → TCP Stream" can reveal the raw conversation, which might show FTP commands, but the protocol column remains unchanged.
3. **Use the "Decode As" feature**: this is the key to forcing Wireshark to interpret the traffic correctly.
   - Right-click a packet and select "Decode As...".
   - In the pop-up window, you'll see fields for the port and protocol.
   - Click the `+` button to add a new decoding rule.
   - Set the port field to the non-standard port being used (e.g., `10008`).
   - Set the "Current" protocol to the actual protocol (e.g., `FTP`).
   - Click OK.
4. **Result**: Wireshark will now re-analyze the traffic and correctly label all packets on that port as the FTP protocol, demonstrating the power of DPI.
