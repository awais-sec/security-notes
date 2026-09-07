# LAN Technologies (Physical Signaling and Addressing, Channel Access)

## Physical Networking Terminology

- **Signal**: the physical representation of data transmitted over the media.
- **Modulation**: the process of combining data with a carrier wave signal for transmission, especially over long distances, creating an analog signal.
  - **Baseband**: a single carrier wave encodes a single data stream. Digital baseband (line coding) encodes data directly into a binary signal.
  - **Broadband**: multiple carrier waves at different frequencies (channels) are transmitted simultaneously over the same medium, each modulated with its own data stream, increasing bandwidth.
- **Demodulation**: the receiver reads and extracts the data from the modulated signal.
- **Modem**: a device that both modulates symbols for transmission and demodulates them from received signals.
- **Bit Rate**: the number of bits per second carried on the network.
  - **Gross bit rate**: the actual bits encoded into the medium.
  - **Net bit rate (throughput)**: useful information after removing overhead (error correction, headers).
  - **Goodput**: throughput minus any generated overhead.
- **Baud Rate**: the number of symbols per second in a transmission, related to network sampling size.
- **Multiplexing**: combining multiple different messages into a unified communication stream.
  - **Multiplexer (MUX)**: combines multiple lower bit rate streams into a single high-capacity link.
  - **Demultiplexer (DEMUX)**: separates the individual streams at the receiving end.
  - **Space-Division Multiplexing (SDM)**: uses multiple parallel physical connectors (e.g., separate wires for audio channels).
  - **Frequency-Division Multiplexing (FDM)**: uses different carrier signals at different frequencies for different streams.
  - **Time-Division Multiplexing (TDM)**: divides each conversation into time slots that take turns using a shared channel.

## Data Link Layer (Layer 2)

Defines the logical network topology operating on top of the physical network.

- **Media Access Control (MAC) Sublayer** (lower half): analogous to physical connections and signals. Packages bits into frames (Layer 2 PDU). Negotiates multiple access to the media when shared. Manages physical MAC addresses unique to each node on the local network.
- **Logical Link Control (LLC) Sublayer** (upper half): performs multiplexing tasks using the MAC sublayer's services.

## Channel Access

Rules for how nodes share the communication medium.

- **Simplex**: information travels in only one direction (A to B).
- **Duplex**: two nodes can exchange information in both directions.
  - **Half-duplex**: traffic can move in both directions, but only one at a time (like a one-lane bridge, collisions can occur).
  - **Full-duplex**: traffic can move in both directions simultaneously (like a two-lane bridge).

## Multiple Access

How multiple nodes share a network segment.

- **Token Passing**: a token is continuously passed through the network; a node holding the token can transmit. Can be a logical ring on any physical topology.
- **Carrier Sense Multiple Access (CSMA)**: used by Ethernet and Wi-Fi. A node listens to the channel (carrier sensing) and transmits if idle. If busy, it waits until clear. No central control needed.

## MAC Addresses (Media Access Control Addresses)

Physical addresses assigned to network interface cards (NICs).

- Usually written as 12 hexadecimal digits grouped in pairs or three groups of four (e.g., `10:0D:7F:F3:CE:8F` or `100D.7FF3.CE8F`).
- The first six digits are the Organizationally Unique Identifier (OUI), identifying the manufacturer.
- The last six digits are the device ID, a unique serial number.
- **Locally Administered Address**: a custom MAC address manually overridden by an administrator; the OUI must begin with "02".
- **EUI-64 (Extended Unique Identifier)**: a 64-bit physical address used in newer network types like IPv6 or Firewire.
- Each Ethernet frame header contains two MAC addresses: the 6-byte destination MAC address and the 6-byte source MAC address.

## Unicast, Broadcast, and Multicast MAC Addresses

- **Unicast Address**: corresponds to a specific recipient.
- **Broadcast Address**: intended to be read by any node that receives it (`FF:FF:FF:FF:FF:FF` in MAC-based networks). The area a broadcast can reach is the broadcast domain, controlled by routers and switches. Excessive broadcasts can cause congestion.
- **Multicast Address**: intended for multiple nodes but not everywhere. The eighth bit of a multicast MAC address is 1 (if the second hexadecimal digit is odd). Higher-level IPv4 multicasts are translated to Ethernet multicasts using the OUI `01:00:5E`. Most switches flood multicast addresses by default.

## Ethernet Frames

The Layer 2 PDU.

**Header** — contains address and control information (4 parts):

- Six-byte MAC destination address.
- Six-byte MAC source address.
- Optional four-byte 802.1Q tag: for VLAN identity and priority.
- Two-byte EtherType value: specifies the protocol of the payload.

**Payload**: the useful data being carried (can be other Layer 2 data or a Layer 3 packet like IP). Length varies: minimum 46 bytes (padding if necessary) and maximum is the Maximum Transmission Unit (MTU) of the network. Standard Ethernet MTU is 1500 bytes, but jumbo frames can have an MTU up to 9000 bytes.

**Frame Check Sequence (FCS)**: a four-byte Cyclic Redundancy Check (CRC) used for error checking. Calculated from the rest of the frame; the receiver repeats the calculation to detect corruption during transit.
