# Advanced Networking Topics

## I. Wireless Networking

### Wireless standards

- The 802.11n, 802.11ac, and 802.11ax standards are the most common today.
- 802.11ac operates only in the 5 GHz range; 802.11ax operates in both 2.4 GHz and 5 GHz ranges.
- 802.11ax is backward-compatible with 802.11a/b/g/n/ac.

### Channels and frequencies

- Channels 1, 6, and 11 are recommended for 2.4 GHz networks because they don't overlap.
- 802.11b signals occupy roughly 30 MHz of spectrum, which causes overlap between adjacent channels.
- 802.11ax supports multiple channels in both 2.4 GHz and 5 GHz bands.

### MIMO and MU-MIMO

- 802.11ac supports up to eight spatial streams (a 100% speed increase over 802.11n).
- 802.11ax uses OFDMA, a multi-user version of OFDM.
- 802.11ax creates MU-MIMO connections for both downlink and uplink transmissions.

### Antennas

- **Omnidirectional**: 360-degree coverage but less range.
- **Directional**: Focused signal path, greater range.

### Wireless network setup

- **Basic Service Set (BSS)**: A single AP connected to the wired network and wireless stations.
- **Extended Service Set (ESS)**: Multiple BSSs forming a single subnetwork.
- **Ad hoc mode (IBSS)**: Direct communication between wireless devices without an AP.

### Troubleshooting

- Signal loss (attenuation) can result from distance, obstacles, or interference.
- Signal-to-noise ratio measures the desired signal against background noise.
- Wireless repeaters can extend transmission distances.

## II. Cloud Computing

### Cloud service models

- **SaaS (Software as a Service)**: Delivering licensed applications over the web; users don't manage infrastructure. Example: web-based email, office applications.
- **PaaS (Platform as a Service)**: Users deploy applications using provider-supported tools, without managing underlying infrastructure. Example: web application hosting, development environments.
- **IaaS (Infrastructure as a Service)**: Provides fundamental computing resources (VMs, storage, networks); users manage the OS, storage, etc. Example: virtual servers, cloud storage.

### Cloud delivery models

- **Private**: Resources owned by the organization.
- **Public**: Resources available to the general public, owned by a third party.
- **Community**: Resources shared by several organizations.
- **Hybrid**: A combination of private and public clouds.

### Key cloud characteristics

On-demand self-service, broad network access, resource pooling, rapid elasticity, and measured service.

### Infrastructure as Code (IaC)

Managing infrastructure via machine-readable definition files.

- **Push method**: The controlling server pushes configurations to the destination.
- **Pull method**: The server pulls configurations from the controlling server.

- **Scalability**: Performance scaled to meet anticipated or real-world demand.
- **Elasticity**: Resources provisioned and released as needed.
- **Multitenancy**: Multiple customers share the same infrastructure.

## III. Network Operations and Documentation

### Importance of documentation

Aids in troubleshooting, inventory management, and knowledge transfer. Should be clear, concise, and understandable for new personnel.

### Essential documentation elements

Floor plans, network topology diagrams (wired and wireless), wiring layouts and rack diagrams, IDF/MDF documentation, server configurations (services, IP addresses, OS), network equipment configurations, performance baselines, DNS/DHCP details, wireless site survey reports, audit/assessment reports, standard operating procedures (SOPs).

### Physical vs. logical diagrams

- **Physical diagrams**: Show the physical construction of the network.
- **Logical diagrams**: Show how the network functions and how data flows.

### Baseline configurations

Performance measurement used for comparison to assess network performance. Should be taken periodically, under the same conditions.

### Policies and procedures

Acceptable Use Policies (AUPs), security policies, data loss prevention (DLP) policies, incident response plans, disaster recovery plans, Memorandum of Understanding (MOU), password policies, backup procedures, notification-of-change procedures.

### Configuration documentation and regulations

Software and hardware configurations must be documented. Regulations carry legal restrictions with legal consequences. Standard labeling rules should be enforced to prevent confusion.

## IV. High Availability and Disaster Recovery

### Backups

- **Full**: Copies all files and directories. Fastest to restore, but takes longest to back up.
- **Differential**: Backs up all changes since the last full backup.
- **Incremental**: Backs up only changes since the last full or incremental backup. Faster backup, but takes longer to restore.
- **Snapshots**: Instantaneous copies of a system at a particular point in time.

### Backup best practices

Offsite storage of backup media, labeling of backup media, verification of successful backups.

### Power and recovery metrics

- **UPS (Uninterruptible Power Supply)**: Ensures data availability during power failures, protects against data loss from power fluctuations.
- **PDU (Power Distribution Unit)**: Distributes electric power.
- **MTBF (Mean Time Between Failures)**: Anticipated time between failures.
- **MTTR (Mean Time to Recovery)**: Time to repair a system after failure.
- **RTO (Recovery Time Objective)**: Maximum acceptable downtime.
- **RPO (Recovery Point Objective)**: Maximum acceptable data loss, measured in time.

### High availability technologies

Fault tolerance (e.g. RAID), load balancing, multipathing, redundant hardware, NIC teaming, port aggregation, clustering.

### Active-active vs. active-passive

- **Active-active**: All devices are in use.
- **Active-passive**: Standby devices remain ready but unused until the primary fails.

### First Hop Redundancy Protocols (FHRP)

Allows a default router address to be configured in case the primary router fails.

- **HSRP (Hot Standby Router Protocol)**: Cisco proprietary FHRP.
- **VRRP (Virtual Router Redundancy Protocol)**: Industry-standard FHRP.

## V. Network Monitoring and Tools

- **Network management**: Monitoring devices, protocols, and usage from a central location.
- **Alerting**: Via email or SMS.
- **Performance monitoring**: Tracking network usage statistics and user trends.
- **Power monitoring**: Identifying and logging power-related problems.
- **Wireless monitoring**: Heat maps, AP discovery, security settings.

### SNMP (Simple Network Management Protocol)

Uses Management Information Bases (MIBs) to define parameters, and Object Identifiers (OIDs) to identify managed objects within MIBs.

### Network testing types

- **Performance testing**: Evaluating performance under normal conditions.
- **Load testing**: Evaluating performance under heavy load.
- **Stress testing**: Evaluating performance under extreme conditions.

### Performance metrics

- **Error rate**: Frequency of errors.
- **Utilization**: Percentage of resources being used.
- **Packet drops**: Number of packets not reaching their destination.

### Logging

- **Log files**: System, security, application, and device logs.
- **Security logs**: Security-related events, such as successful/unsuccessful logon attempts.
- **Syslog**: A message logging standard for UNIX/Linux systems, with severity levels ranging from 0 (emergency) to 7 (debugging).
- **Network analyzers**: Used to find weaknesses in existing networks.

## VI. Addressing, Routing, and Switching

- **IP addressing**: Each system must have a unique IP address to communicate on a network. The IP address identifies both the network and the specific node.
- **Routing tables**: The means by which data is directed through the network; must stay up to date and complete.

### Routing metrics

- **Hop count**: Number of hops to reach a node.
- **MTU**: Maximum transmission unit.
- **Bandwidth**: Specifies maximum packet size for transmission.
- **Cost**: A value associated with traveling from A to B.

### VLANs (Virtual Local Area Networks)

Logically separate networks. Increase security, reduce broadcast storms, and ease the burden on routers.

- **Auto-MDI-X (Auto-Medium-Dependent Interface Crossover)**: Automatically detects if a crossover is needed.
- **Trunking**: Using multiple network connections for better speed.

## VII. Network Devices

- **Firewalls**: Control access to protect from outside threats.
- **Switches**: Learn MAC addresses and forward data to the appropriate port, improving performance by creating direct paths between devices. Can operate in full-duplex mode.
  - **Cut-through switching**: Forwards packets immediately.
  - **Store-and-forward switching**: Error-checks the entire packet before forwarding.
  - **Fragment-free switching**: Reads enough of the packet to determine collisions.
- **MDI-X ports**: Standard ports on hubs, switches, and routers.
- **Wireless LAN controllers**: Used for wireless authentication.
- **Load balancers**: Distribute workload among servers, increasing redundancy and data availability.

### Three-tiered architecture

- **Core layer**: The backbone; switches and routers manage separate networks.
- **Distribution/aggregation layer**: Boundary layer with Layer 3 switches, manages data between VLANs.
- **Access/edge layer**: Connects to end devices.

### Spine and leaf model

The spine is the backbone; leaf switches interconnect all the devices.

### Traffic flows

- **East-West**: Data flowing among devices within a specific data center.
- **North-South**: Data flowing in and out of the data center.

### Shared storage

- **SAN (Storage Area Network)**: Block-level access to storage.
- **NAS (Network Attached Storage)**: File-level access to storage.

## VIII. Cabling and Connectors

### Twisted-pair cabling

- **UTP (Unshielded Twisted Pair)**: Commonly used in networks.
- **STP (Shielded Twisted Pair)**: More resistant to interference.

Category ratings:

| Category | Speed |
|---|---|
| Cat 5 | 100 Mbps |
| Cat 5e | 1 Gbps |
| Cat 6 | 10 Gbps |
| Cat 6a | 10 Gbps and beyond |
| Cat 7 | 10 Gbps and beyond |
| Cat 8 | 40 Gbps up to 30m |

### Coaxial cabling

- **RG-59**: Low-power video, not for long distances.
- **RG-6**: Cable TV and cable modems.
- **Twinaxial (Twinax)**: Short distances with SFP transceivers.

### Fiber optic cabling

Uses light signals for data transmission.

- **Multimode fiber**: Shorter distances, 50-micron core / 125-micron cladding.
- **Single-mode fiber**: Longer distances, 8.3-micron core / 125-micron cladding.

### Plenum vs. PVC cables

- **Plenum**: Fire-resistant, doesn't produce toxic fumes.
- **PVC**: Standard cabling.

### Media connectors

RJ-45 for twisted pair, F-type for coaxial, SC/ST for fiber optic. **FDP (Fiber Distribution Panel)**: For termination, storage, and splicing of fiber connections.

### Ethernet standards

| Standard | Speed / Medium |
|---|---|
| 10BASE-T | 10 Mbps, RJ-45 |
| 100BASE-TX | Fast Ethernet over twisted pair |
| 100BASE-FX | Fast Ethernet over fiber optic |
| 100BASE-SX | Lower-cost fiber alternative to 100BASE-FX |
| 1000BASE-T (Gigabit Ethernet) | Over Cat 5 or better UTP |
| 10GBASE-T | 10 Gbps over Cat 6/6a |
| 40GBASE-T | 40 Gbps over Cat 8 |

### Wavelength Division Multiplexing (WDM)

- **DWDM (Dense WDM)**: Multiple signals combined onto a single fiber.
- **CWDM (Coarse WDM)**: More relaxed standards, often for TV cabling.

### Troubleshooting tools

Tone generator and locator, wire crimpers, punchdown tools.

### Data rate units

Kbps, Mbps, Gbps (bits per second) vs. KBps, MBps, GBps (bytes per second).

**Throughput**: Actual data transfer rate, may be lower than bandwidth. Speed test sites are used to determine actual internet connection speed.
