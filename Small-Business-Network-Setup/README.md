# Project 1: Small Business Network Setup

## Overview
A simulated small business network built in Cisco Packet Tracer, demonstrating VLAN segmentation, trunking between switches, and inter-VLAN routing using Router-on-a-Stick.

## Objective
Design and configure a network for a small business with three departments (Sales, IT, Guest) spread across two switches, with full connectivity between all VLANs through a central router.

## Topology

```
                Router0
                   |
              Gig0/1 (trunk, all 3 VLANs)
                   |
     Switch0 (Fa0/24 trunk) ---- Switch1 (Fa0/24 trunk)
      /      |      \                 |      |      \
   PC1     PC2     PC0              PC3    PC4     PC5
```

Router0 connects to Switch0 via a trunk link carrying three VLANs (Sales, IT, Guest). Switch0 and Switch1 are interconnected by a second trunk, allowing each VLAN to span both switches. Inter-VLAN routing is handled by the router using subinterfaces (Router-on-a-Stick).

- Router0 connects to Switch0 only (Router-on-a-Stick) — no direct link to Switch1
- Switch0 and Switch1 are connected via a trunk link (Fa0/24) so VLANs span both switches
- Each VLAN has members on both switches
- Laptop0 exists in the topology but is not connected to any device (not part of this design)

## VLAN & IP Addressing Plan

Subnets were sized using VLSM based on realistic host requirements per department.

| VLAN | Name  | Subnet            | Gateway        | Devices                  |
|------|-------|--------------------|----------------|--------------------------|
| 10   | Sales | 192.168.1.0/26     | 192.168.1.1    | PC1, PC3                 |
| 20   | IT    | 192.168.1.64/27    | 192.168.1.65   | PC2, PC4                 |
| 30   | Guest | 192.168.1.96/28    | 192.168.1.97   | PC0, PC5                 |

## Port Reference

**Switch0**

| Port | Connected to | Mode | VLAN |
|---|---|---|---|
| Gig0/1 | Router0 (Gig0/1) | Trunk | 10, 20, 30 |
| Fa0/24 | Switch1 (Fa0/24) | Trunk | 10, 20, 30 |
| Fa0/1 | PC1 | Access | 10 (Sales) |
| Fa0/2 | PC2 | Access | 20 (IT) |
| Fa0/10 | PC0 | Access | 30 (Guest) |

**Switch1**

| Port | Connected to | Mode | VLAN |
|---|---|---|---|
| Fa0/24 | Switch0 (Fa0/24) | Trunk | 10, 20, 30 |
| Fa0/3 | PC3 | Access | 10 (Sales) |
| Fa0/4 | PC4 | Access | 20 (IT) |
| Fa0/5 | PC5 | Access | 30 (Guest) |

**Router0**

| Port | Type |
|---|---|
| Gig0/1 | Trunk (802.1Q) to Switch0 |
| Gig0/1.10 | Subinterface, VLAN 10, IP 192.168.1.1/26 |
| Gig0/1.20 | Subinterface, VLAN 20, IP 192.168.1.65/27 |
| Gig0/1.30 | Subinterface, VLAN 30, IP 192.168.1.97/28 |

## Skills Demonstrated
- VLAN creation and port assignment across multiple switches
- Trunk configuration (802.1Q) between switches and router
- Router-on-a-Stick (subinterfaces for inter-VLAN routing)
- VLSM subnetting to right-size each department's address space
- Verification using `show vlan brief`, `show ip route`, and `ping` tests between VLANs

## Configuration Files
- [`configs/router-config.txt`](configs/router-config.txt)
- [`configs/switch0-config.txt`](configs/switch0-config.txt)
- [`configs/switch1-config.txt`](configs/switch1-config.txt)

## Tools Used
- Cisco Packet Tracer
