# Network Security and Troubleshooting

## Network Security

- Wireless networks are vulnerable to security threats; data protection and authentication are critical. Hackers exploit weaknesses for unauthorized access.
- **Rogue Access Points (APs)**: unauthorized APs connected to a network without approval, set up by hackers or unaware employees. Allow attackers to intercept sensitive traffic, cause network congestion, and expose users to malware/phishing. Mitigation strategies include using Wireless LAN Controllers (WLC) to manage APs, monitoring with LWAPP/CAPWAP, implementing Radio Resource Management (RRM) to detect rogue APs, and preventing workstations from connecting to unknown APs. Vigilance is crucial.
- Security in MAN involves firewalls, data encryption, intrusion detection systems (IDS), and virtual private networks (VPNs).
- Security requirements for networks include firewalls, encryption, access control lists (ACLs), user authentication (e.g., multi-factor), and data protection.

### Network Segmentation

Divides a network into smaller, isolated sections. Benefits include improved security (limits access), better performance (reduces congestion), easier management, and containment of threats.

- **Types**: physical segmentation (separate hardware), logical segmentation (VLANs and subnets), and micro-segmentation (SDN).
- **VLANs** group devices logically without physical separation.
- **Subnetting** divides a large network into smaller sub-networks for efficient IP allocation and reduced congestion.
- **Firewalls** regulate traffic between segments.
- A **DMZ (Demilitarized Zone)** is an isolated segment for public-facing services.
- **Software-Defined Networking (SDN)** uses software to control traffic flow and enable flexible segmentation.
- **Zero Trust** enforces strict access controls, and network segmentation supports this model.
- **Implementing segmentation** involves identifying assets, defining policies, using VLANs/subnets/firewalls, and monitoring traffic.
- **Challenges**: complex setup and increased management overhead.
- **Best practices**: regular review, automation, and least privilege access.

## Troubleshooting

The process of finding the cause of a problem and its solution. Troubleshooting methodology generally involves identification, diagnosis, and resolution.

### Network+ Troubleshooting Model (CompTIA)

1. Identify the problem.
2. Establish a theory of probable cause.
3. Test the theory to determine the cause.
4. Establish a plan of action to resolve and identify potential effects.
5. Implement the solution or escalate as necessary.
6. Verify full system functionality and implement preventative measures.
7. Document findings, actions, and outcomes.

### Identifying Problems

Gather information, question users, identify symptoms, determine changes, duplicate if possible, approach individually.

### Establishing Probable Cause

Question the obvious, consider multiple approaches (top-down, bottom-up, divide and conquer, follow the path, spot differences, move the problem), keep track of attempts.

### Creating an Action Plan

Quantify changes, identify needs (tools, access, help), create a step-by-step plan (with phases), consider side effects, create a back-out plan.

### Implementing the Solution

Schedule time, ensure readiness, be prepared to escalate or roll back.

### Verifying Problem Resolution

Verify the original problem is gone, check for side effects, apply preventative measures, go back to planning/diagnosis if needed, undo unnecessary changes.

### Documenting Outcomes

Issue, underlying cause, resolution steps, complications, organization policies.

### Lessons Learned

Extent of problem (scope, cost, duration), adequacy of response, responder preparedness, promptness of correction, clarity of communication/documentation, potential improvements (training, policies, technical changes).

## Troubleshooting Physical Connectivity (Copper)

Breaks in continuity, internal/external interference (noise), wrong wire order, split pairs (mismatched wire pairs at termination), physical interface failures (NIC, hub, switch transceiver).

**Typical copper problems**: short, open, attenuation, bad cable/connector, EMI/RFI, crosstalk, split pairs, transposed TX/RX, wrong termination standard, network hardware failure, speed/duplex mismatch.

## Troubleshooting Optical Connections

Attenuation, bad cable or dirty connector (ends must be very clean; clean and polish), bend radius limitation (straighten or replace), mismatched connection/wavelength/fiber (replace incompatible optics/cable), network hardware failure (diagnose/replace transceiver or device), managing optical link budget (accounting for losses from connectors, fiber length, splices, and safety margin).

## Cable Testing Tools

- **Multimeter**: measures current, voltage, resistance.
- **Tone generator and probe**: identifies cables.
- **Cable tester**: checks continuity and wiring order.
- **Time-Domain Reflectometer (TDR)**: locates faults by sending signals and analyzing reflections.
- **Spectrum analyzer**: analyzes signal frequencies.
- **Cable certifier**: verifies cable meets specific standards.
- **Loopback plug**: tests network interface functionality.
- Electrostatic shock can damage electronics; use anti-static wrist straps.
