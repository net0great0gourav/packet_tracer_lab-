# packet_tracer_lab-
CCNA networking labs built in Cisco Packet Tracer
# Inter-VLAN Routing (Router-on-a-Stick) in Cisco Packet Tracer

## Goal
Connect 3 VLANs across two switches using a trunk link, and let the VLANs communicate with each other through a single router (router-on-a-stick).

## Topology
- 2 switches (2960), 1 router (2911), 6 PCs
- Each switch has one PC from each VLAN
- Switch0 and Switch1 are connected by an 802.1Q trunk
- Switch1 connects to the router through a trunk

![Topology](<2026-11-06 (1).png>)

## IP Plan
| VLAN | Network | Gateway | PCs |
|------|---------|---------|-----|
| 10 | 192.168.10.0/24 | 192.168.10.1 | PC0, PC3 |
| 20 | 192.168.20.0/24 | 192.168.20.1 | PC1, PC4 |
| 30 | 192.168.30.0/24 | 192.168.30.1 | PC2, PC5 |

## What I configured
- Created VLANs 10, 20 and 30 on both switches
- Assigned access ports so each switch has one PC per VLAN
- Configured trunk ports between the switches and from Switch1 to the router
- Created router subinterfaces with `encapsulation dot1Q` and a gateway IP for each VLAN
- Set the default gateway on every PC

## Router configuration
```
interface gig0/0
 no shutdown
interface gig0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
interface gig0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
interface gig0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
```
![ROUTER INFO](<2026-11-06 (17).png>)

## Problems I hit and how I fixed them
1. **Same-VLAN pings timed out across switches.** I had assigned the PC ports on Switch1 to the wrong access ports. I checked with `show vlan brief`, then moved each port into the correct VLAN.
2. **Router interface was down.** The link between the router and Switch1 showed red. The router port was shut down by default, and `no shutdown` fixed it.
3. **Pings between different VLANs failed at first.** (edit: write what you found and fixed, for example a missing default gateway or a subinterface setting. Be honest and specific.)

## What I learned
- A trunk allows all VLANs by default, and `switchport trunk allowed vlan` can limit which ones cross it
- VLANs separate traffic, so a router is needed for communication between them
- `show vlan brief`, `show interfaces trunk` and `show ip interface brief` are the quickest ways to find where a problem is

## Verification
- Same-VLAN ping across the trunk (PC0 to PC3): successful
- Different-VLAN ping through the router (PC0 to PC2): successful

![Ping test](<2026-11-06 (10).png>)
![show vlan brief](<2026-11-06 (15).png>)
![show interfaces trunk](show-trunk.png)

## Next steps
DHCP, ACLs, NAT and OSPF labs will be added to this repository.
