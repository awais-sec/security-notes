# Ethernet Standards

Both RJ-45 (copper) and SC (fiber) connectors can be found on switches.

## Legacy Ethernet (10 Mbps)

- **10BASE-T**: 10 Mbps, up to 100 meters over Category 3 (2 twisted pairs). First popular twisted-pair standard.
- **10BASE2 ("Thinnet")**: used RG-58 coaxial cable, simpler but more limited than Thicknet (10BASE5).

## Fast Ethernet (100 Mbps)

- **100BASE-TX**: 100 Mbps, up to 100 meters over Category 5 (2 twisted pairs). Still common in small networks.
- **100BASE-FX**: 100 Mbps, up to 412m (half-duplex) or 2km (full-duplex) over MMF at 1300nm. Can also use SMF for 10km range.
- **100BASE-SX**: 100 Mbps, up to 300 meters over MMF at 850nm. An inexpensive alternative to 100BASE-FX.

## Gigabit Ethernet (1 Gbps)

Became available starting in 1999, dominant in consumer NICs and small hardware. Requires full-duplex operation.

- **1000BASE-T**: 1 Gbps, up to 100 meters over Category 5 or higher (requires all 4 twisted pairs for simultaneous send/receive). Often incorrectly called 1000BASE-TX.
- **1000BASE-LX**: 1 Gbps, up to 500m (MMF) or 5km (SMF) at 1300nm–1310nm. Can be used on both fiber types.
- **1000BASE-SX**: 1 Gbps, up to 550 meters over MMF at 850nm. Range varies by cable grade.

## 10 Gigabit Ethernet (10 Gbps)

Fastest widely adopted standard today. Does not support half-duplex.

- **10GBASE-T**: 10 Gbps, up to 55m on Cat6, 100m on Cat6a (uses all 4 twisted pairs).
- **10GBASE-SR**: 10 Gbps, 33m–400m over MMF at 850nm (range depends on cable grade).
- **10GBASE-LR**: 10 Gbps, up to 10 km over SMF at 1310nm.
- **10GBASE-SW**: 10 Gbps, 33m–400m over MMF at 850nm. Used for transmission on SONET WAN equipment (Carrier Ethernet).

## Specialty Ethernet Standards

- **IEEE 1905.1-2013**: defines convergent networks carrying Ethernet traffic over multiple media types.
- **Ethernet over power line**: speeds listed up to 1 Gbps, but practical speeds may be lower. Can be faster and longer range than home Wi-Fi without custom cabling.
- **Carrier Ethernet**: used by ISPs on a neighborhood or city-wide level over Gigabit or faster optical links (e.g., using 10GBASE-SW).

## 40 Gigabit Ethernet

- **40GBASE-T**: 40 Gbps, up to 30m over Category 8 (4 twisted pairs).
