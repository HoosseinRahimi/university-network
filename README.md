# University Campus Network

A repaired and fully tested university campus network scenario for Cisco Packet Tracer 8.2.

## Included files

- `university-network-fixed.pkt` — final Packet Tracer project
- `university-network-final-report.md` — architecture, repairs, addressing plan, and 25 test cases
- `university-network-presentation-speech.txt` — ready-to-read presentation speech

## Network features

- VLANs for management, administration, engineering, dormitory wireless, and servers
- Inter-VLAN routing and HSRP virtual gateways
- OSPF dynamic routing
- DHCP and DNS services
- Internal and simulated external web servers
- NAT-based simulated Internet access
- Two autonomous wireless access points

## Final validation

- Topology health: healthy
- Down links: 0
- Duplicate IP addresses: 0
- Final test cases: 25 passed
- All wired and wireless clients reached their gateways, internal services, and the simulated Internet

## Packet Tracer startup note

Allow several seconds for the wireless clients to scan and associate after opening the project. If a laptop temporarily shows a `169.254.x.x` address, open **Desktop → IP Configuration** and click **DHCP** once after association.
