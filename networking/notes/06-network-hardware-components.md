# Network Components (Hardware)

- **Nodes**: devices like computers, servers, and printers connected to the network.
- **Transmission media**: cables (wired) or radio waves (wireless) that carry data.

## Networking Devices

- **Hub**: a Layer 1 (Physical Layer) device that connects multiple nodes in a star configuration. Acts as a repeater with multiple ports, broadcasting received signals on all other ports. Less efficient than switches due to lack of data filtering and prone to collisions within its collision domain. Rarely used today. Can be passive (simply connects), active (regenerates signals), or intelligent (basic management).
- **Repeater**: a Layer 1 device that extends the range of network signals by regenerating and amplifying weak signals weakened by attenuation over distance. Can be used in wired and wireless networks. Simple repeaters amplify noise as well; digital repeaters regenerate a clean signal.
- **Network Tap**: a device that allows monitoring of network traffic.
  - **Passive tap**: physical splice that sends the signal in two directions (listens only, minimal impact).
  - **Active tap**: contains a repeater to maintain signal strength (can interact with the network).
- **Bridge**: a Layer 2 (Data Link Layer) device that links two or more Layer 1 segments (collision domains) into a larger network. Filters traffic by reading MAC addresses and forwarding frames only to the destination segment, reducing collisions and congestion. Maintains a MAC table to learn the location of MAC addresses. If the destination MAC is unknown, it floods the frame to all other segments except the source.
- **Translating Bridge**: a Layer 2 device that can join segments using different Layer 2 protocols (e.g., Ethernet to Token Ring or Wi-Fi) by translating frame formats. Requires more processing power.
- **Switch**: typically refers to a Layer 2 switch, which is essentially a bridge with three or more ports. Forwards data based on MAC addresses. Provides full-duplex communication, reducing network collisions. Modern networks primarily use switches due to their efficiency.
  - **Managed switches**: offer advanced features like VLAN support, monitoring, and security features.
  - **Unmanaged switches**: plug-and-play with minimal configuration.
  - **Layer 3 switches** (multilayer switches): can examine frame payloads and understand higher-layer (Network Layer) information for more advanced routing and control. Switches operate within a broadcast domain, as they forward broadcast frames.
- **Router**: operates at Layer 3 (Network Layer) and directs data (packets) between different networks. Uses IP addresses to determine the best path for data. Connects LANs to the internet and other external networks. Sits at the boundaries between broadcast domains. Requires more computing functions and memory than a typical switch. Each NIC of a router has a different MAC address and belongs to a different broadcast domain. Routers exchange information about network conditions to determine the quickest paths.
- **Gateway**: typically used to describe any connection between multiple networks at any OSI level, or as a synonym for "router." More technically, every IP device's configuration includes a default gateway — a router to send packets destined outside its subnet. Can also be a specific type of router with more complex duties like joining networks with different protocols or addressing schemes, or connecting a LAN to a WAN. Example: VoIP gateway for phone and internet calls.
- **Modem**: converts digital data to analog signals (modulation) for transmission over analog media (e.g., telephone lines, cable) and vice versa (demodulation). Enables internet access via telephone lines (DSL modem), cable (cable modem), or fiber (fiber modem). A device that both encodes and decodes symbols is called a modem.
- **Access Point (AP)**: provides wireless connectivity (Wi-Fi) within a network. Acts as a bridge between Wi-Fi and the wired LAN. Supports multiple devices and enhances network coverage. Can be standalone or integrated into routers. Uses Wi-Fi standards (802.11 a/b/g/n/ac/ax).
- **Network Interface Card (NIC)**: a hardware component in devices (computers, laptops, networked devices) that connects them to a network. Can be wired (Ethernet NIC) or wireless (Wi-Fi NIC). Converts digital data into radio signals for wireless transmission.
- **Firewall**: a security device (hardware or software) that monitors and controls incoming and outgoing network traffic based on pre-defined security rules. Protects against cyber threats and unauthorized access (hacking, malware, phishing, data breaches). Used in DMZ (Demilitarized Zone) networks to protect internal networks.
- **Wireless Repeater/Extender**: extends Wi-Fi range. Repeaters amplify and rebroadcast the signal, while extenders create new access points. Useful for covering dead zones.
- **Gateway (as in VoIP Gateway)**: acts as a bridge between different network types or protocols, converting data formats for seamless communication (e.g., for phone and internet calls).
- **Load Balancer**: distributes network traffic across multiple servers to ensure smooth performance and prevent overload, used in high-traffic websites and cloud environments.
- **Proxy Server**: acts as an intermediary between users and the internet, enhancing security and privacy by masking IP addresses. Used for content filtering and caching.
- **Wireless Controller**: manages multiple access points in a large wireless network, providing centralized configuration, monitoring, load balancing, and security enforcement. Uses protocols like Lightweight Access Point Protocol (LWAPP) or CAPWAP to manage APs, and can implement Radio Resource Management (RRM) to detect rogue APs.
- **Wireless Bridge**: connects two wired networks wirelessly, used to link buildings or remote offices where cable access is unavailable. Acts as a point-to-point connection.
