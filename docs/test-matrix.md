# Test Matrix

The following tests were performed in Cisco Packet Tracer to validate routing, redundancy, Layer 2 failover, access-control policies, and service availability.

| ID | Test | Procedure | Expected Result | Observed Result | Status |
|---|---|---|---|---|---|
| T01 | OSPF adjacency and route learning | Run `show ip ospf neighbor` and `show ip route` on R1 | OSPF neighbors reach FULL state and internal VLAN routes are learned dynamically | R1 established FULL adjacencies with R2, SW1 and SW2. VLAN networks were learned through OSPF, with alternative equal-cost next hops visible in the routing table | PASS |
| T02 | HSRP gateway failover | Shut down `interface Vlan30` on SW1 and check `show standby brief` on SW2 | SW2 should assume the Active role for HSRP group 30 while preserving virtual gateway `192.168.30.1` | SW2 transitioned from Standby to Active for VLAN 30 and retained virtual gateway `192.168.30.1` | PASS |
| T03 | EtherChannel member-link failure | Shut down SW1 `FastEthernet0/23` and run `show etherchannel summary` | Port-channel1 should remain operational through the remaining physical member | `Fa0/23` changed to Down while `Fa0/24` remained bundled; `Po1(SU)` remained in use | PASS |
| T04 | Rapid STP uplink failover | On SW4, shut down the active root-port uplink `Fa0/1` and run `show spanning-tree vlan 20` | The alternate uplink should transition to Root/Forwarding | `Fa0/2` changed from Alternate/Blocking to Root/Forwarding and connectivity path reconverged | PASS |
| T05 | Student VLAN Web access | From PC-STUD1, open the internal Web server at `192.168.40.10` | HTTP access should be allowed by `ACL_VLAN30_IN` | Internal Campus Web portal loaded successfully | PASS |
| T06 | Student VLAN FTP restriction | From PC-STUD1, attempt `ftp 192.168.40.12` | FTP access should be denied by `ACL_VLAN30_IN` | FTP connection timed out and was not established | PASS |

## Evidence

Relevant validation screenshots are available in the [`assets`](../assets/) directory:

- [`ospf-validation.png`](../assets/ospf-validation.png)
- [`hsrp-failover.png`](../assets/hsrp-failover.png)
- [`etherchannel-failover.png`](../assets/etherchannel-failover.png)
- [`rstp-failover.png`](../assets/rstp-failover.png)
- [`acl-validation-web.png`](../assets/acl-validation-web.png)
- [`acl-validation-ftp.png`](../assets/acl-validation-ftp.png)

## Recovery

Interfaces deliberately disabled during failover testing were restored with `no shutdown` after validation so that the `.pkt` file included in this repository represents the fully operational topology.
