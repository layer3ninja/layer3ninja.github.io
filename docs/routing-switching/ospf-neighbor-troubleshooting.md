---
title: OSPF Neighbor Troubleshooting
description: Why OSPF adjacencies get stuck in INIT, 2-WAY, EXSTART or EXCHANGE — and how to fix each.
tags:
  - OSPF
  - Troubleshooting
  - CCNP
---

# OSPF Neighbor Troubleshooting

An OSPF adjacency has to pass through a fixed sequence of states. **The state it gets stuck in
tells you which class of problem you have.**

```mermaid
flowchart LR
  Down --> Init --> TwoWay[2-Way] --> ExStart --> Exchange --> Loading --> Full
```

## Quick reference

| Stuck in | Most likely cause | Check with |
|---|---|---|
| *(no neighbor at all)* | Hellos not sent/received: interface passive, ACL, wrong network statement, L2 issue | `show ip ospf interface brief` |
| **INIT** | One-way Hellos — the neighbor doesn't see *our* Router ID in its Hello (ACL, unicast/multicast filtering) | `debug ip ospf hello` |
| **2-WAY** | Normal between two DROTHERs on a broadcast segment. A problem only if one side has priority 0 on both ends | `show ip ospf neighbor` |
| **EXSTART / EXCHANGE** | **MTU mismatch** (classic), or duplicate Router IDs | `show ip ospf interface` |
| **LOADING** | Corrupted / rejected LSAs, LSR not answered | `debug ip ospf adj` |

## Hello parameters that must match

For two routers to become neighbors these must match on the link:

- Area ID (and area type — stub flag)
- Subnet and mask (on broadcast / point-to-point with non-/32)
- Hello and Dead timers
- Authentication type and key
- Network type compatibility (e.g. broadcast ↔ point-to-point will form but won't exchange routes correctly)

Router IDs must be **unique**.

## Useful commands

```text
show ip ospf neighbor
show ip ospf interface GigabitEthernet0/1
show ip ospf interface brief
show ip protocols
debug ip ospf adj
debug ip ospf hello
```

## Example: MTU mismatch (stuck in EXSTART)

```text
R1# show ip ospf neighbor
Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   EXSTART/DR      00:00:36    10.0.12.2       GigabitEthernet0/1
```

Fix — make the IP MTU match on both sides (preferred), or as a workaround:

```text
R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip mtu 1500
! workaround only — hides the real mismatch:
R1(config-if)# ip ospf mtu-ignore
```

!!! warning "Prefer fixing the MTU"
    `ip ospf mtu-ignore` lets the adjacency form, but the underlying MTU mismatch can still
    drop large packets (and large LSUs) on that link.

## References

- RFC 2328 — OSPF Version 2, §10.1 (Neighbor states)
- Cisco — *Troubleshoot OSPF Neighbor Problems* (Cisco TAC tech note)
