# IP Addressing, Subnetting, and NAT

## Addressing Overview

- IP addressing is essential for network communication.
- Each device on a network requires a unique identifier (IP address).
- IPv4 addresses are 32-bit numbers divided into four octets (e.g., 192.168.1.1).
- IPv6 is a newer protocol with 128-bit addresses written in hexadecimal (e.g., 2001:db8::), allowing a much larger address space. IPv6 supports auto-configuration and enhanced security. Its adoption is increasing due to IPv4 exhaustion.
- Without IP addressing, devices cannot communicate over a network.

## IPv4 Address Types

- **Unicast**: one-to-one communication to a specific host. Used in most applications like web browsing and email. Reduces network congestion compared to broadcasting.
- **Multicast**: one-to-many communication; only subscribed hosts receive the traffic. Used in streaming, video conferencing, online gaming, and routing protocols. Reduces bandwidth usage compared to broadcasting. Example multicast address: 224.0.0.1 (all hosts in a subnet).
- **Broadcast**: one-to-all communication within a network segment. Layer 2 broadcasts use MAC address `FF:FF:FF:FF:FF:FF` within a LAN. Layer 3 broadcasts use the highest address in the network range (e.g., 192.168.1.255) and are used in ARP, DHCP, and routing protocol advertisements. Excessive broadcast traffic can cause network congestion.
- **Special addresses**:
  - **Loopback (127.0.0.1)**: used for testing local network functions without actual network connectivity.
  - **Default Route (0.0.0.0)**: used as a destination when no other specific route is known.
  - **Limited Broadcast (255.255.255.255)**: sent to all devices on the directly connected network.

## Network and Host Address

- **Network Address**: identifies a network (e.g., 172.16.0.0).
- **Host Address**: identifies a specific device within that network (e.g., 172.16.1.10).
- Their combination uniquely defines a device.
- Subnetting helps organize networks efficiently.

## IPv4 Address Classes (Legacy)

Now largely replaced by CIDR.

| Class | Range (first octet) | Hosts per network | Example | Typical use |
|---|---|---|---|---|
| A | 1–126 | ~16 million | 10.0.0.0/8 | Large organizations and ISPs |
| B | 128–191 | ~65,000 | 172.16.0.0/16 | Medium-sized organizations |
| C | 192–223 | 254 | 192.168.1.0/24 | Most common in LANs |
| D | 224–239 | — | — | Multicast |
| E | 240–255 | — | — | Reserved for future/experimental use |

## Classless Inter-Domain Routing (CIDR)

A method for allocating IP addresses and routing IP packets that replaced classful addressing. Allows for more flexible IP address assignments and more efficient use of IP addresses.

- **CIDR notation**: IP Address/Prefix Length (e.g., 192.168.10.0/24). The prefix length indicates the number of bits used for the network portion of the address.
- **Benefits**: more efficient use of IPs, more and smaller subnets.
- **Subnet mask**: helps determine the network and host portions of an IP address.
  - Example: a /24 subnet mask is 255.255.255.0.
  - Example: 192.168.10.0/26 has a subnet mask of 255.255.255.192, resulting in 64 total addresses and 62 usable host addresses.
- Subnetting involves breaking down an IP address into smaller blocks (subnets). Network ID + Host ID = Full IP Address.

## Reserved (Private) IP Address Ranges

Used for internal networks, requiring NAT for internet access. Prevent IP conflicts with public networks.

| Class | Range |
|---|---|
| A | 10.0.0.0 – 10.255.255.255 |
| B | 172.16.0.0 – 172.31.255.255 |
| C | 192.168.0.0 – 192.168.255.255 |

## Network Address Translation (NAT)

Modifies IP addresses in IP packet headers while in transit. Used to conserve IP addresses and share a single public IP address across many private devices.

**Types of NAT**

- **Static NAT**: fixed one-to-one mapping between a private IP address (Inside Local) and a public IP address (Inside Global). Example: 192.168.1.10 maps to 203.0.113.5.
- **Dynamic NAT**: maps private IPs to a pool of available public IPs (Inside Global) dynamically.
- **PAT (Port Address Translation)**: uses a single public IP address (Inside Global) for multiple internal devices (Inside Local) by distinguishing them using different port numbers.

**Understanding NAT names**

- **Inside Local Address**: private IP address inside the local network. Example: 192.168.1.10.
- **Inside Global Address**: the public IP address mapped to the inside local address when communicating with the outside world. Example: 203.0.113.1 (for PAT).
- **Outside Local Address**: the remote network's view of the internal device (often the inside global address).
- **Outside Global Address**: the public IP address of a device on the internet.

**How NAT works**: the NAT router changes the source private IP address to the public IP address when traffic leaves the local network, and maintains a translation table to return incoming traffic to the correct internal device based on the destination port and IP.

**Benefits of NAT**: conserves IP addresses, improves network security by hiding internal IPs, simplifies network management.
