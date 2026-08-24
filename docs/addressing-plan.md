# IP Addressing Plan

This document summarizes the VLAN, gateway, Layer 3 transit, management, and server addressing used in the Cisco Packet Tracer topology.

## VLANs and HSRP Gateways

| VLAN | Name | Subnet | HSRP Virtual Gateway | SW1 SVI | SW2 SVI |
|---|---|---|---|---|---|
| 10 | Management | 192.168.10.0/24 | 192.168.10.1 | 192.168.10.2 | 192.168.10.3 |
| 20 | Admin | 192.168.20.0/24 | 192.168.20.1 | 192.168.20.2 | 192.168.20.3 |
| 30 | Students | 192.168.30.0/24 | 192.168.30.1 | 192.168.30.2 | 192.168.30.3 |
| 40 | Internal Servers | 192.168.40.0/24 | 192.168.40.1 | 192.168.40.2 | 192.168.40.3 |
| 50 | DMZ | 192.168.50.0/24 | 192.168.50.1 | 192.168.50.2 | 192.168.50.3 |

SW1 is configured with HSRP priority 110 and preemption, making it the preferred active gateway under normal operating conditions.

## Layer 3 Transit Networks

| Link | Network | Endpoint A | Endpoint B |
|---|---|---|---|
| R1 - R2 | 10.0.0.0/30 | R1: 10.0.0.1 | R2: 10.0.0.2 |
| R1 - SW1 | 10.0.1.0/30 | R1: 10.0.1.1 | SW1: 10.0.1.2 |
| R1 - SW2 | 10.0.1.4/30 | R1: 10.0.1.5 | SW2: 10.0.1.6 |
| R2 - SW1 | 10.0.1.8/30 | R2: 10.0.1.9 | SW1: 10.0.1.10 |
| R2 - SW2 | 10.0.1.12/30 | R2: 10.0.1.13 | SW2: 10.0.1.14 |

All Layer 3 transit links participate in OSPF area 0.

## OSPF Router IDs

| Device | Router ID |
|---|---|
| R1 | 1.1.1.1 |
| R2 | 2.2.2.2 |
| SW1 | 11.11.11.11 |
| SW2 | 22.22.22.22 |

## Infrastructure Management Addresses

| Device | Management IP |
|---|---|
| SW3 | 192.168.10.11 |
| SW4 | 192.168.10.12 |
| SW5 | 192.168.10.21 |
| SW6 | 192.168.10.22 |
| SW-DC | 192.168.10.30 |

Default gateway for management devices: `192.168.10.1`.

## Server Addressing

| Server | VLAN | IP Address | Service |
|---|---:|---|---|
| WEB | 40 | 192.168.40.10 | HTTP |
| DNS | 40 | 192.168.40.11 | DNS |
| FTP | 40 | 192.168.40.12 | FTP |
| EMAIL | 40 | 192.168.40.13 | SMTP / POP3 |
| WEB-DMZ | 50 | 192.168.50.10 | HTTP / HTTPS |

## Addressing Design

User and service VLANs use `/24` networks for clarity and ease of administration in the lab environment. Point-to-point Layer 3 transit connections use `/30` networks to provide two usable addresses per link.
