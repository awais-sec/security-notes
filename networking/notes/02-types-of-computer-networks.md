# Types of Computer Networks (Scope)

## LAN (Local Area Network)

Connects computers and devices within a limited geographic area, such as a home, office, school, or campus.

**Features**

- Limited geographical coverage: typically confined to a single building or nearby buildings.
- High speed: offers high data transfer speeds, ranging from 10 Mbps to 10 Gbps or more.
- Low latency: minimal delay in data transmission due to short distances.
- Private ownership: usually owned and managed by individuals or organizations.
- Wired and wireless: can be Ethernet-based (wired) or Wi-Fi-based (wireless).

**Types**

- **Wired LAN**: uses Ethernet cables and switches for data transmission; common standard is IEEE 802.3.
- **Wireless LAN (WLAN)**: uses radio signals instead of cables; common standard is Wi-Fi (IEEE 802.11).
- **Virtual LAN (VLAN)**: a logically segmented LAN within a larger network for better management and security.

**Components**

- **Network devices**: computers and servers (end devices), switches (manage traffic), routers (connect to external networks), access points (APs) (for wireless), network interface cards (NIC) (for connectivity).
- **Transmission media**: wired (Ethernet cables like CAT5, CAT6, fiber optic) and wireless (Wi-Fi, Bluetooth, infrared).
- **Protocols**: Ethernet (IEEE 802.3), Wi-Fi (IEEE 802.11), TCP/IP, DHCP (dynamic IP assignment), DNS (domain name resolution).

**Advantages**

- High-speed communication: faster data transfer than WANs.
- Resource sharing: sharing printers, files, and internet connections.
- Cost-effective: reduces hardware costs through shared resources.
- Centralized data management: facilitates easy backup and security.
- Security: more secure as it is limited to a small area.

**Disadvantages**

- Initial setup cost: requires investment in hardware and installation.
- Security risks: vulnerable if not properly secured.
- Limited range: operates only within a confined space.
- Network management complexity: requires technical knowledge.

**Common LAN technologies**: Ethernet (IEEE 802.3), Wi-Fi (IEEE 802.11). Token Ring (IEEE 802.5) is an older, now obsolete technology.

**Applications**: home networks (personal devices), educational institutions (internet access, resources), corporate offices (communication, file sharing), hospitals (digital records, equipment). A beginning network technician is most likely to install or maintain a LAN.

## MAN (Metropolitan Area Network)

Spans a city or a large campus, covering a geographical area larger than a LAN but smaller than a WAN (range: 5 to 50 km). Designed for high-speed connectivity and data exchange for organizations, universities, and businesses within a metropolitan region.

**Characteristics**

- Medium geographic coverage: city or large campus.
- High-speed connectivity: 100 Mbps to 10 Gbps.
- Interconnects multiple LANs: acts as a bridge.
- Wired and wireless technologies: fiber optics, coaxial cables, or wireless.
- Public or private ownership: government, university, ISP, or private organization.

**Components**: routers, switches, modems, gateways.

**Transmission media**: fiber optic cables, coaxial cables, microwave links, radio links (WiMAX).

**Technologies**: fiber optic, SONET (Synchronous Optical Network), Ethernet MAN, WiMAX (Worldwide Interoperability for Microwave Access), MPLS (Multiprotocol Label Switching), FDDI (Fiber Distributed Data Interface).

**Advantages**: high-speed data transmission, interconnects multiple LANs, cost-effective (shared infrastructure), supports multiple services (VoIP, video, cloud), reliable and scalable.

**Disadvantages**: high setup and maintenance costs, complex network management, security risks, signal interference (wireless).

**Applications**: university and college campuses, corporate networks (multiple branches), government and public services, ISPs and telecom networks, banking and financial institutions.

**Security considerations**: firewalls, data encryption, intrusion detection systems (IDS), virtual private networks (VPNs).

**Example**: implementing a MAN in a university network involves planning, equipment deployment (core routers, APs, fiber), subnetting/VLAN configuration (MPLS for prioritization), and security implementation (firewalls, VPNs, IDS).

## WAN (Wide Area Network)

A telecommunications network that extends over a large geographical area, often connecting multiple cities, countries, or even continents. Used by businesses, governments, and organizations to connect their branches and remote users.

**Characteristics**

- Covers large geographical areas.
- Uses public or private networks for connectivity.
- Relies on leased lines, satellites, fiber optics, and wireless connections.
- Provides communication between distant locations.

**WAN vs. LAN vs. MAN**: differs in coverage area, speed, ownership, technology, and cost.

**Components**: routers, switches, modems, gateways.

**Transmission media**: fiber optic cables, microwave links, satellite communication, leased lines.

**Types of WAN connections**

- **Circuit-switched WAN**: dedicated communication path (e.g., PSTN).
- **Packet-switched WAN**: divides data into packets (e.g., Internet, MPLS); more efficient and cost-effective.
- **Wireless WAN (WWAN)**: uses radio signals and satellites (e.g., cellular networks 3G, 4G, 5G, satellite internet).

**Advantages**: enables global connectivity, facilitates business communication and data sharing, provides remote access to resources, uses advanced security protocols.

**Disadvantages**: high installation and maintenance costs, slower speed compared to LANs, vulnerable to security threats, requires complex network management.

**Real-life applications**: banking networks (ATMs, online banking), e-commerce (global platforms), educational institutions (remote learning), military and government (secure communication).

## PAN (Personal Area Network)

The smallest type of network, designed for personal use over a short-range area (usually within 10 meters). Typically connects personal devices such as smartphones, laptops, tablets, smartwatches, and wireless peripherals (keyboards, mice, printers). Mostly wireless, using technologies like Bluetooth, Zigbee, NFC, and Infrared (IR).

**Features**: small area coverage, personal use, low power consumption, wireless connectivity, simple network architecture.

**Advantages**: convenience (seamless connection), low cost (no expensive infrastructure), energy efficient, easy to use (simple setup).

**Disadvantages**: limited range, interference issues (other wireless devices), security risks (Bluetooth, NFC hacking), low data transfer rate (compared to LAN/WAN).

**Real-life applications**: smartphones and accessories (Bluetooth headsets, smartwatches), contactless payments (NFC), gaming consoles (wireless controllers), smart home devices (Zigbee), automotive systems (Bluetooth infotainment).
