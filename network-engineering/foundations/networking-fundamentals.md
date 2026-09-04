# Networking Fundamentals

## Introduction

Networking is connecting devices to share data and resources, including both physical and virtual connections. It's vital for communication, the internet, business operations, and personal connectivity.

The internet is a collection of interconnected networks that exchange information using common standards. It isn't owned by any single entity. Local networks come in various sizes, from small home networks to large corporate networks, facilitating resource sharing and internet access.

### Types of networks by scale

- **Small home networks**: Connect a few computers to each other and the internet.
- **Small office/home office (SOHO) networks**: Allow home or remote offices to connect to corporate networks or access shared resources.
- **Medium to large networks**: Used by corporations and schools, can have many locations with hundreds or thousands of interconnected hosts.
- **World Wide networks**: The internet is a network of networks connecting hundreds of millions of computers worldwide.

## Mobile and Connected Devices

The internet connects a wide range of devices:

- **Mobile devices**: Smartphones, tablets, smartwatches, smart glasses
- **Connected home devices**: Security systems, appliances, smart TVs, gaming consoles
- **Other connected devices**: Smart cars, RFID tags, sensors, actuators, medical devices

## Data Transmission

- **Bit**: The smallest unit of data, represented as 0 or 1.
- **Byte**: A group of 8 bits.

### Transmission methods

- **Electrical signals**: Data represented as electrical pulses on copper wire.
- **Optical signals**: Electrical signals converted into light pulses (fiber optic).
- **Wireless signals**: Use infrared, microwave, or radio waves.

### Bandwidth vs. throughput

- **Bandwidth**: Capacity of a medium to carry data, measured in bits per second (bps): Kbps (thousands), Mbps (millions), Gbps (billions).
- **Throughput**: Actual amount of data transferred over a given time, influenced by data volume, data type, and latency. The slowest link in a network path limits overall throughput.

## Network Topologies

**Topology**: A network's physical and logical layout.

- **Physical topology**: Actual layout of cables and devices.
- **Logical topology**: How the network appears to devices.

### Common topologies

- **Bus**: All devices connect to a single backbone. Simple and low cost, but a single break disrupts the entire network.
- **Ring**: Data travels in a circular fashion. Easy troubleshooting, but a break in the ring can disrupt the network.
- **Star**: All devices connect to a central hub or switch. Easy to troubleshoot and expand, but the central device is a single point of failure.
- **Mesh**: Every device connects to every other device, providing redundancy. Highly reliable but expensive and complex.
- **Hybrid**: Combination of different topologies.

### Wireless topologies

- **Infrastructure**: Wireless devices connect to a wired network through an access point (AP).
- **Ad hoc**: Devices communicate directly with each other without an AP.
- **Mesh**: Wireless nodes are interconnected, providing multiple paths for data. Self-healing, scalable, and reliable.

## Network Types

- **Peer-to-Peer**: No dedicated servers, all devices act as both clients and servers. Easy to set up and less complex, but not as secure and not scalable.
- **Client/Server**: Dedicated servers provide services to clients.
- **LAN (Local Area Network)**: Restricted to a single geographic location, high-speed, low cost.
- **WLAN (Wireless LAN)**: Uses radio frequency (RF) signals, flexible, can augment or replace wired LANs.
- **WAN (Wide Area Network)**: Spans multiple geographic locations, slower than LANs, more expensive.
- **MAN (Metropolitan Area Network)**: A WAN confined to a specific geographic area, such as a city.
- **CAN (Campus Area Network)**: Multiple LANs within a limited geographic area, like a university campus.
- **SAN (Storage Area Network)**: Networked storage devices.
- **PAN (Personal Area Network)**: Connects devices in close proximity, often using Bluetooth or infrared.
- **SDWAN (Software-Defined WAN)**: Extends SDN principles to WANs.
- **MPLS (Multiprotocol Label Switching)**: High-performance WAN technology using labels for data routing.
- **mGRE (Multipoint Generic Routing Encapsulation)**: Extends GRE tunneling capabilities.

## WAN Link Types

| Technology | What it is | Advantage / Disadvantage | Example |
|---|---|---|---|
| DSL (Digital Subscriber Line) | High-speed internet over existing telephone lines | ADSL: faster download than upload speeds | Home internet via a phone jack |
| Cable Modem | Uses cable TV infrastructure | Faster than DSL for many users, but shared bandwidth can slow during peak hours | Internet from a cable TV provider |
| PSTN (Public Switched Telephone Network) | Traditional voice telephone network | Low bandwidth, mostly obsolete for internet use | Dial-up internet |
| Leased Lines (T-carrier/E-carrier) | Dedicated high-speed digital connections | Reliable and secure, but very expensive | Businesses using T1/E1 lines |
| Metro-Optical Networks | Fiber-optic networks serving metropolitan areas | High-speed | A city's broadband infrastructure |
| SONET/SDH | High-speed fiber-optic WAN technology | | Telecoms transmitting large volumes of data between cities |
| PON (Passive Optical Network) | Fiber-optic network using unpowered splitters | Cost-effective and energy-efficient | FTTH services like Google Fiber |
| DWDM (Dense Wavelength-Division Multiplexing) | Multiple optical signals on one fiber via different wavelengths | Maximizes fiber-optic capacity | Long-distance undersea cable transmission |
| Satellite Internet | Internet via orbiting satellites | Available in remote areas, but high latency and weather-affected | Starlink, HughesNet |

## Termination Points

- **Demarcation Point**: Connection point between the ISP's network and the customer's network.
- **Demarc Extension**: Extends the demarcation point to a more convenient location.
- **Smart Jacks**: Network interface devices at the demarcation point, providing signal conversion, testing, and surge protection.
- **CSU/DSU (Channel Service Unit/Data Service Unit)**: Translates data formats between LAN and WAN.

## Virtual Networking

- **Virtualization**: Emulating physical hardware and software in a software environment.
- **Hypervisor**: Software that manages virtual machines.
  - **Type I**: Bare metal, runs directly on hardware.
  - **Type II**: Hosted, runs on top of an operating system.
- **NFV (Network Function Virtualization)**: Virtualizes network services, allowing them to run on standard servers.
- **Virtual network components**: vNICs, virtual routers, virtual switches, shared memory, virtual CPUs, storage.

## Clients and Servers

- **Hosts**: Devices that participate in network communication.
- **Clients**: Request and display information from servers.
- **Servers**: Provide information and services to clients.
- **Peer-to-Peer (P2P)**: Networks where devices act as both clients and servers. Advantages: easy setup, less complexity, lower cost. Disadvantages: no centralized administration, less security, not scalable, slower performance.

## Network Infrastructure Components

- **End devices**: Devices users interact with, such as computers, printers, phones.
- **Intermediate devices**: Connect end devices and manage data flow, including switches, routers, and wireless access points.
- **Network media**: Physical channels for data transmission, including copper and fiber optic cables, and wireless.

## ISP Connectivity

- **ISP (Internet Service Provider)**: Provides internet access.
- **Common connection options**: Cable, DSL, cellular, satellite, dial-up.
- **Router**: Essential for secure connection to the internet; provides switching, wireless access, IP addressing, and security.

## OSI Model

Conceptual framework for network communication, divided into seven layers:

1. **Physical (Layer 1)**: Defines physical characteristics like cables, connectors, and voltage.
2. **Data Link (Layer 2)**: Handles error detection, correction, and MAC addressing.
3. **Network (Layer 3)**: Responsible for routing and logical addressing (IP).
4. **Transport (Layer 4)**: Manages data flow, segmentation, and error checking (TCP, UDP).
5. **Session (Layer 5)**: Establishes, manages, and terminates connections between applications.
6. **Presentation (Layer 6)**: Handles data formatting, encryption, and decryption.
7. **Application (Layer 7)**: Provides network services to applications.

### Data encapsulation

- **Encapsulation**: Adding headers and trailers to data as it moves down the OSI model.
- **Decapsulation**: Removing headers and trailers as data moves up the OSI model.

## Ports and Protocols

- **Protocols**: Sets of rules for network communication.
- **Connection-oriented protocols**: Guarantee data delivery (TCP).
- **Connectionless protocols**: Best-effort delivery, no guarantee (UDP).

### Common protocols

| Protocol | Purpose |
|---|---|
| IP | Connectionless, provides addressing and routing |
| TCP | Connection-oriented, reliable data delivery |
| UDP | Connectionless, lightweight, efficient |
| ICMP | Error checking and reporting (ping) |
| IPSec | Secure communication via encryption and authentication |
| FTP | File transfer, unsecure |
| SSH | Secure remote access and file transfer (SFTP) |
| SFTP | Secure version of FTP, using SSH |
| Telnet | Unsecure remote access |
| SMTP | Sending email |
| DNS | Resolves hostnames to IP addresses |
| DHCP | Assigns IP addresses to clients automatically |
| TFTP | Simple file transfer, unsecure |
| HTTP | Accessing web pages |
| HTTPS | Secure version of HTTP |
| NTP | Synchronizes time between devices |
| POP3 | Receiving email |
| IMAP4 | Receiving email, more secure than POP3 |
| SNMP | Managing network devices |
| LDAP | Accessing directory services |
| SMB | File sharing and printing in Windows networks |
| Syslog | Logging protocol used in UNIX/Linux systems |
| SMTPS (SMTP TLS) | Secure version of SMTP using TLS |
| LDAPS | Secure version of LDAP over SSL |
| SQL | Communicating with databases |
| RDP | Remote access to Windows systems |
| SIP | Establishing and managing VoIP calls |

## Network Services

- **DNS (Domain Name System)**: Resolves hostnames to IP addresses. Uses a hierarchical namespace; DNS records store domain information.
- **DHCP (Dynamic Host Configuration Protocol)**: Assigns IP addresses automatically. The DHCP server maintains a pool of IP addresses; clients request addresses from the server.
- **NTP (Network Time Protocol)**: Synchronizes time between devices, using a hierarchical system of time servers.
