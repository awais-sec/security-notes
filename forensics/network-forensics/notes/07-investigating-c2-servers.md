# Investigating Command-and-Control (C2) Servers

## What is a C2 Server?

A Command-and-Control (C2) server is the infrastructure an attacker uses to send commands to and receive data from compromised machines (bots) within a botnet. It's the central point from which an attacker orchestrates their campaign.

## C2 Communication Techniques

Attackers use various methods to hide their C2 traffic and evade detection:

- **Domain Generation Algorithms (DGAs)**: malware algorithmically generates a large number of pseudo-random domain names. The malware then attempts to contact these domains in sequence, and the attacker only needs to register one of them in advance to establish a connection, making the C2 infrastructure resilient to takedown.
- **Fast Flux**: a DNS technique where a single domain name is associated with a rapidly changing pool of IP addresses, making it difficult for defenders to block the C2 server by simply blacklisting an IP.
- **Blending In**: attackers craft their malicious traffic to mimic legitimate protocols and services (e.g., disguising C2 traffic as normal-looking HTTP or DNS requests) to blend in with regular network noise and avoid raising alarms.

## Identifying C2 Traffic

Investigators can hunt for C2 activity by looking for specific network patterns:

- **Beaconing**: malware often "calls home" to its C2 server at regular, periodic intervals to check for new commands. This regular, repeated pattern of connections to the same external host is a strong indicator of a C2 channel and can be spotted in traffic analysis (e.g., IO graphs showing periodic spikes).
- **DNS Query Analysis**: investigators specifically look for queries to known malicious domains, or for signs of a DGA, such as a high volume of queries for unusual, algorithmically generated-looking domain names.
