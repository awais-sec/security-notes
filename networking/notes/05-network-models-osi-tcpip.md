# Network Models

## Purpose of Reference Models

Reference models use abstraction layers to separate the roles of hardware, software, and other elements within a system. Two primary models are OSI and TCP/IP.

## The OSI Model (Open Systems Interconnection)

Started in 1977 by the International Organization for Standardization (ISO). Goal: interoperability — allowing products and interfaces from many vendors and networks to communicate seamlessly by separating network functions into seven abstraction layers:

1. **Physical Layer**: transmits raw bits over a physical medium (cables, wireless). Data unit: bits.
2. **Data Link Layer**: provides error-free data transfer between adjacent network nodes. Handles MAC addressing. Sublayers: MAC (Media Access Control) and LLC (Logical Link Control). Data unit: frame (or sometimes cell).
3. **Network Layer**: responsible for routing packets across networks. Handles IP addressing. Data unit: packet.
4. **Transport Layer**: provides reliable or unreliable end-to-end data delivery. Handles TCP and UDP. Data unit: segment (TCP) or datagram (UDP).
5. **Session Layer**: manages and controls connections between applications. Data unit: data.
6. **Presentation Layer**: handles data formatting, encryption, and compression. Data unit: data.
7. **Application Layer**: provides network services to end-user applications (e.g., HTTP, FTP, SMTP). Data unit: data.

## The TCP/IP Model (Internet Protocol Suite)

Refers to its primary protocols: Transmission Control Protocol (TCP) and Internet Protocol (IP). Used by almost every modern network.

**Layers (typically a 4-layer model)**:

1. **Link Layer** (or Network Interface Layer): combines the Physical and Data Link layers of the OSI model. Handles physical transmission and MAC addressing.
2. **Internet Layer** (or Network Layer): responsible for routing packets across networks using IP.
3. **Transport Layer** (or Host-to-Host Layer): provides end-to-end data transport using protocols like TCP and UDP. TCP and UDP are the Transport layer protocols that carry network data.
4. **Application Layer**: combines the Session, Presentation, and Application layers of the OSI model. Provides application services (e.g., HTTP, FTP).

TCP/IP communications involve concrete interactions between protocols.

**Robustness Principle**: "Be conservative in what you do, be liberal in what you accept from others."

## OSI vs. TCP/IP

- OSI is a theoretical model focused on interoperability through layered functions.
- TCP/IP is a practical model based on the concrete implementation of protocols.
