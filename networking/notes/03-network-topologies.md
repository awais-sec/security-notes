# Network Topologies

**Topology definition**: how the nodes of a network are connected to one another.

- **Point-to-point**: a single line joining two nodes for direct communication.
- **Bus**: connects every node to a single shared cable (the bus). An example of a point-to-multipoint connection.
- **Ring**: all nodes are arranged in a continuous circle or loop, and data is transmitted in one direction.
- **Star**: most modern LANs use this, with a central hub or switch having connections radiating outward to every node. Also called hub-and-spoke in WANs.
- **Mesh**: each node connects independently to multiple other nodes.
  - **Full mesh**: every node connects to every other node (complex and expensive).
  - **Partial mesh**: has enough connections for redundancy without a full mesh.
  - A mesh topology would be used when fault tolerance is the greatest concern.
- **Hybrid**: combines elements of multiple other topologies.
- **Logical topologies**: depend on the protocols the network uses. Examples include Switched Ethernet, Shared Ethernet, and Token Ring.
