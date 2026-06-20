# Lab 04 – OSPF Multi-Router Network

## Objective
Demonstrates dynamic routing using OSPF across three routers connected via 
serial WAN links, each representing a separate site. Routers automatically 
learn about remote networks without manual static routes.

## Topology
![Topology Diagram](topology.png)

## Devices Used
- 3x Router (Cisco 1941 with HWIC-2T serial modules)
- 3x PC

## IP Addressing Plan

| Link/Network        | Subnet           |
|----------------------|-------------------|
| Router0 ↔ Router1    | 10.0.12.0/30      |
| Router0 ↔ Router2    | 10.0.13.0/30      |
| Router1 ↔ Router2    | 10.0.23.0/30      |
| Site A LAN           | 192.168.1.0/24    |
| Site B LAN           | 192.168.2.0/24    |
| Site C LAN           | 192.168.3.0/24    |

## Key Configurations

\`\`\`
interface Serial0/1/0
 ip address 10.0.12.1 255.255.255.252
 clock rate 64000
 no shutdown

router ospf 1
 network 192.168.1.0 0.0.0.255 area 0
 network 10.0.12.0 0.0.0.3 area 0
 network 10.0.13.0 0.0.0.3 area 0
\`\`\`

## Verification
- `show ip ospf neighbor` confirms all three routers formed OSPF neighbor 
  relationships over the serial links.
- `show ip route` confirms OSPF-learned (O) routes to remote LANs on each router.
- PC0 successfully pings PC1 and PC2, confirming end-to-end connectivity 
  across all three sites via dynamically learned routes.

## What I Learned / Real-World Application
This lab reflects how multi-site organisations connect branch offices over 
WAN links and rely on dynamic routing protocols instead of static routes, 
which don't scale. Troubleshooting DCE/DTE clocking issues on serial links 
gave me a deeper appreciation for why WAN links can fail even when IP 
addressing is correct — a useful diagnostic lesson for real environments.
