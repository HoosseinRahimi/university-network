# University Network — Final Repair and Validation Report

**Project file:** `university-network-fixed.pkt`  
**Validation date:** 25 July 2026  
**Packet Tracer version:** 8.2.0  
**Final topology:** 26 devices, 29 physical links

## 1. Final status

The repaired university network is operational. The final live health scan reported:

- Topology health: **Healthy**
- Down links: **0**
- Duplicate IP addresses: **0**
- All six user clients can reach their VLAN gateway, the internal web server, and the simulated Internet.
- DNS name `www.university.local` resolves and responds successfully.
- DHCP, DNS, internal HTTP, external HTTP, HSRP gateways, OSPF routing, and NAT are operational.

The health tool lists some “cabled without IP” interfaces. These are expected Layer-2 switchports and AP bridge ports; they do not require individual IP addresses and are not faults.

## 2. Repairs performed

### Switching and VLANs

- Recreated and verified VLANs 10, 20, 30, 40, and 50.
- Corrected access-port VLAN assignments.
- Corrected trunk links between access, distribution, core, and server switches.
- Removed the faulty second inter-core link after the original asymmetric EtherChannel caused a Layer-2 loop.
- Retained one stable inter-core trunk on FastEthernet0/23.

### Gateway redundancy and routing

- Restored HSRP virtual gateways ending in `.1` for the campus VLANs.
- Restored inter-VLAN routing on the multilayer core switches.
- Restored OSPF routing between the core and edge network.
- Restored the edge default route and ISP return routing.
- Restored NAT for campus-to-Internet traffic.

### Addressing and services

- Rebuilt and enabled four DHCP pools:
  - `Management_APs`
  - `Admin_Staff`
  - `Engineering_Students`
  - `Dorm_Wireless`
- Restored DHCP addressing for the four wired PCs.
- Enabled DNS and restored the A record:
  - `www.university.local` → `10.1.50.20`
- Enabled the internal and external HTTP services.

### Wireless network

- Replaced the nonfunctional WLC/LAP arrangement with two autonomous `AccessPoint-PT` access points.
- Connected both APs to VLAN 40 access ports.
- Successfully associated both wireless clients, one with each AP.
- Recreated both laptops with clean wireless modules so their default wireless profiles persist.
- Both laptops auto-associate after the wireless scan converges and receive valid VLAN 40 addresses from DHCP.
- Packet Tracer 8.2.0 does not reliably persist scripted custom SSID profiles, so the operational SSID is `Default`. This does not affect connectivity.

## 3. Addressing summary

| Function | Network/address |
|---|---|
| Management/AP VLAN 10 | `10.1.10.0/24`, virtual gateway `10.1.10.1` |
| Administration VLAN 20 | `10.1.20.0/24`, virtual gateway `10.1.20.1` |
| Engineering VLAN 30 | `10.1.30.0/24`, virtual gateway `10.1.30.1` |
| Dorm wireless VLAN 40 | `10.1.40.0/23`, virtual gateway `10.1.40.1` |
| Server VLAN 50 | `10.1.50.0/24`, virtual gateway `10.1.50.1` |
| Core-to-edge transit | `10.255.1.0/30` and `10.255.1.4/30` |
| Edge-to-ISP transit | `203.0.113.0/30` |
| Simulated Internet | `198.51.100.0/24` |
| DHCP server | `10.1.50.10` |
| DNS server | `10.1.50.11` |
| Internal web server | `10.1.50.20` |
| Internet test server | `198.51.100.10` |

## 4. Final test cases

| ID | Test | Expected result | Actual result | Status |
|---:|---|---|---|:---:|
| TC-01 | Live topology health scan | Healthy, no down links | Healthy, 0 down links | PASS |
| TC-02 | Duplicate-IP scan | No duplicates | 0 duplicate IPs | PASS |
| TC-03 | PC-Admin1 → `10.1.20.1` | 4 replies | 4/4, 0% loss | PASS |
| TC-04 | PC-Admin2 → `10.1.20.1` | 4 replies | 4/4, 0% loss | PASS |
| TC-05 | PC-Engineering1 → `10.1.30.1` | 4 replies | 4/4, 0% loss | PASS |
| TC-06 | PC-Engineering2 → `10.1.30.1` | 4 replies | 4/4, 0% loss | PASS |
| TC-07 | Wireless-Client1 → `10.1.40.1` | 4 replies | 4/4, 0% loss | PASS |
| TC-08 | Wireless-Client2 → `10.1.40.1` | 4 replies | 4/4, 0% loss | PASS |
| TC-09 | PC-Admin1 → internal web `10.1.50.20` | Reachable | 4/4, 0% loss | PASS |
| TC-10 | PC-Admin2 → internal web `10.1.50.20` | Reachable | 4/4, 0% loss | PASS |
| TC-11 | PC-Engineering1 → internal web `10.1.50.20` | Reachable | Retest 4/4, 0% loss | PASS |
| TC-12 | PC-Engineering2 → internal web `10.1.50.20` | Reachable | Retest 4/4, 0% loss | PASS |
| TC-13 | Wireless-Client1 → internal web `10.1.50.20` | Reachable | 4/4, 0% loss | PASS |
| TC-14 | Wireless-Client2 → internal web `10.1.50.20` | Reachable | 4/4, 0% loss | PASS |
| TC-15 | PC-Admin1 → Internet server `198.51.100.10` | Reachable through NAT | 4/4, 0% loss | PASS |
| TC-16 | PC-Admin2 → Internet server `198.51.100.10` | Reachable through NAT | Retest 4/4, 0% loss | PASS |
| TC-17 | PC-Engineering1 → Internet server `198.51.100.10` | Reachable through NAT | 4/4, 0% loss | PASS |
| TC-18 | PC-Engineering2 → Internet server `198.51.100.10` | Reachable through NAT | 4/4, 0% loss | PASS |
| TC-19 | Wireless-Client1 → Internet server `198.51.100.10` | Reachable through NAT | 4/4, 0% loss | PASS |
| TC-20 | Wireless-Client2 → Internet server `198.51.100.10` | Reachable through NAT | 4/4, 0% loss | PASS |
| TC-21 | PC-Admin1 → `www.university.local` | Resolve to internal server and reply | 4/4, 0% loss | PASS |
| TC-22 | DHCP service state | Four required named pools enabled | All four required pools present and issuing addresses | PASS |
| TC-23 | Internal/external HTTP state | Both enabled | Both enabled | PASS |
| TC-24 | Wireless association | Both clients associated | Each client associated with a different AP | PASS |
| TC-25 | NAT operation | Dynamic translation entries created | 24 entries observed during testing | PASS |

Some first-attempt Packet Tracer pings lost an initial packet while ARP tables were being populated. Immediate retests returned 4/4 replies; this is normal simulation convergence and not persistent packet loss.

## 5. Suggested live demonstration

1. Open `university-network-fixed.pkt` and allow a few seconds for wireless scanning.
2. Show VLAN separation and the two core switches.
3. From PC-Admin1, ping `10.1.20.1`.
4. From PC-Engineering1, ping `10.1.50.20`.
5. From Wireless-Client1, ping `198.51.100.10`.
6. From PC-Admin1, ping `www.university.local`.
7. Explain that the `.1` addresses are HSRP virtual gateways and that NAT is performed at the edge router.

If a laptop still shows a `169.254.x.x` address immediately after opening the file, wait for its AP association and click **DHCP** once under **Desktop → IP Configuration**. This is a Packet Tracer startup-timing behavior; both laptops obtained correct `10.1.40.x/23` leases during final testing.

## 6. Conclusion

The scenario now meets its functional objectives: segmented campus VLANs, redundant gateway addressing, routed communication between departments, centralized services, wireless dorm access, and simulated Internet connectivity. All final functional test cases passed.
