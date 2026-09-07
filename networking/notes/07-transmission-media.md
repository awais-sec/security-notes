# Transmission Media (Physical Connections)

## Wired (Copper Media)

### Twisted-Pair Cables

Each balanced pair of wires is twisted to reduce electromagnetic interference (EMI) and crosstalk from external sources and adjacent cables. Different twist rates for each pair within a cable further reduce crosstalk. Subject to attenuation (signal weakening over distance).

- **UTP (Unshielded Twisted-Pair)**: relies solely on the twisted-pair effect for interference reduction.
- **Shielded Twisted-Pair (STP)**: uses shielding (foil or braided copper) around individual pairs (U/FTP), the outer cable (F/UTP, S/UTP), or both (F/FTP, S/FTP, SF/FTP) for additional EMI protection. Requires grounding.
- **Categories (Cat)**: defined by the ISO/IEC 11801 standard based on maximum transmission frequency.

  | Category | Max Frequency | Typical Use |
  |---|---|---|
  | Cat3 | 16 MHz | Early Ethernet, voice |
  | Cat5 | 100 MHz | Fast/Gigabit Ethernet, superseded by Cat5e |
  | Cat5e | 100 MHz | Enhanced Cat5 |
  | Cat6 | 250 MHz | Gigabit Ethernet, short 10 Gigabit, often shielded |
  | Cat6A | 500 MHz | Shielded, full distance 10 Gigabit |
  | Cat7 | 600 MHz | Screened and shielded, higher noise resistance |
  | Cat7A | 1000 MHz | Potential for 40 Gigabit |
  | Cat8.1/8.2 | 2000 MHz | Shielded, 25/40 Gigabit for data centers, shorter distances |

- **Connectors**: modular RJ-45 connectors are most common for twisted-pair Ethernet. Described by number of positions (P) and contacts (C). Typically male-to-male plugs on cables and female jacks on devices/wall drops. UTP couplers (female-to-female) can join two cables.
- **Termination standards**: T568A and T568B define the wiring order of the eight wires in an RJ-45 connector. Straight-through cables use the same standard at both ends (for connecting different types of devices), while crossover cables use T568A at one end and T568B at the other (for connecting similar types of devices). Console cables (e.g., RJ-45 to DB9 RS-232 (Yost), Mini-USB to USB-A, RJ-45 to USB-A) have different connectors on each end for device configuration.
- **Centralized connections**: in structured cabling, cables terminate on a distribution frame using a punchdown block (e.g., 66, 110, Krone, BIX). A special punchdown tool is used. Patch panels provide a more flexible way to manage connections between equipment and the distribution frame.
- **Termination tools**: snips, cable stripper, cable crimper, punchdown tool. Proper wire order is crucial during termination.

### Coaxial Cables

Have a central copper core surrounded by insulation (dielectric), a braided or foil shield, and an outer jacket. Impedance is a key characteristic (typically 50 or 75 ohms).

- **RG standards**:

  | Standard | Impedance | Use |
  |---|---|---|
  | RG-59 | 75 ohms | Baseband video, older cable TV, not reliable for broadband network |
  | RG-6 | 75 ohms | Digital cable, satellite, cable modem |
  | RG-11 | 75 ohms | Longer distance RG-6 applications |
  | RG-8 | 50 ohms | 10BASE5 "Thicknet" Ethernet, obsolete |
  | RG-58 | 50 ohms | 10BASE2 "Thinnet" Ethernet, obsolete |

- **Connectors**: BNC connectors (used with T connectors for legacy Ethernet) and F connectors (used for cable TV and modem connections) are common.
- **Termination**: requires coaxial termination tools (stripper, crimper or compression tool). Involves cutting layers, folding back the braid, and attaching the connector.

### Twinaxial (Twinax) and Triaxial Cables

Twinax has a balanced pair of conductors with a dielectric and shield. Triaxial adds an extra layer of insulator and a second shield for better interference resistance.

## Wired (Optical Media)

### Optical Fibers

Transmit data as light pulses through thin strands of glass or plastic. Immune to EMI.

- **Fiber types**: single-mode fiber (SMF) for long distances, multi-mode fiber (MMF) for shorter distances. Different grades of MMF exist with varying ranges.
- **Wavelength-Division Multiplexing (WDM)**: transmits multiple data streams simultaneously at different wavelengths of light over a single fiber. CWDM (Coarse WDM) supports fewer channels with wider spacing, while DWDM (Dense WDM) supports many channels with narrow spacing.
- **Optical fiber connectors**: various types like SC, LC, ST, MTP/MPO.
- **Optical splicing**: joining two fiber ends. Mechanical splicing uses a device to hold fibers together temporarily. Fusion splicing heats and melts the tips into a permanent connection.
- **Fiber termination**: attaching connectors to fiber ends. Methods include adhesive/polish (epoxy), quick termination connectors (factory-polished stub with mechanical splice), and using pigtails (factory-terminated cable spliced to the field cable).
- **Couplers and splitters**: fiber couplers (a type of splitter) fuse multiple fibers at one end to a single core, allowing one input to multiple outputs (similar to coaxial splitters). Combining multiple inputs of the same wavelength is not possible due to interference.
- **Modular transceivers**: pluggable modules like GBIC, SFP, XFP, SFP+, QSFP that convert electrical signals to optical and vice versa, used in network devices for fiber connections. SFP family transceivers are generally smaller than GBIC.
- **Media converters**: convert the physical signaling between different media types (e.g., copper Ethernet to fiber).

## Wireless Media

- Uses radio waves to transmit data.
- Higher frequencies generally allow for faster speeds but shorter range, while lower frequencies offer slower speeds but longer range.
- Wi-Fi (IEEE 802.11 standards) is a common technology for wireless LANs. Various standards (a, b, g, n, ac, ax) offer different speeds and features.
- Other wireless technologies for PANs include Bluetooth, Zigbee, NFC, and Infrared (IR).
