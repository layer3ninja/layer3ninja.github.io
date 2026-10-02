---
title: Overlapping Subnets Across an IPsec Tunnel
description: Two sites, one /24, and a tunnel that couldn't route. How NAT (and sometimes PBR) gets around overlapping address space.
tags:
  - IPsec
  - NAT
  - PBR
  - VPN
  - Cisco IOS
---

# Overlapping Subnets Across an IPsec Tunnel

<!-- EDIT ME: add one or two sentences of real context, e.g. "During an acquisition..." or
     "A partner wanted a site-to-site VPN...". Keep employer/partner names out. -->

We needed a site-to-site IPsec tunnel between two networks, and both sides were using the
same private range. The tunnel itself was easy. Getting traffic through it was not.

    When the **same subnet exists on both ends** of a VPN, routing alone can't fix it.
    Hosts never even send the traffic to their gateway. The fix is **NAT**: present each
    side to the other as a unique "virtual" network. **PBR** only helps when the overlap is
    *partial* (a route conflict), not when the subnets are identical.

![Overlapping subnets across an IPsec tunnel, translated with NAT on Router A](../assets/images/overlapping-subnets-ipsec.svg)

## The situation

| | Site A (us) | Site B (remote) |
|---|---|---|
| LAN | `192.168.1.0/24` | `192.168.1.0/24` |
| VPN peer | Router A | Router B |

Re-addressing either side wasn't an option in the time we had, so the overlap had to be
handled on the network.

## Why it breaks

!!! danger "Three things go wrong at once"
    1. **Hosts don't route it.** Host A wants to reach `192.168.1.50`. That's in its own
       subnet, so it ARPs locally instead of sending to the gateway. The packet never
       reaches the router, let alone the tunnel.
    2. **The router can't route it.** Router A has `192.168.1.0/24` as a *connected* route.
       A connected route always beats anything learned for the remote side.
    3. **The crypto ACL is ambiguous.** `permit ip 192.168.1.0/24 192.168.1.0/24` is
       meaningless: source and destination are the same network.

So the goal is simple: **neither host may ever send to an address in its own subnet.**

## Option 1: NAT (the real fix for identical subnets)

Router A translates in **both directions**, so each side sees the other as a unique network:

- Site A (`192.168.1.0/24`) is presented to Site B as **`10.5.5.0/24`**
- Site B (`192.168.1.0/24`) is presented to Site A as **`10.10.10.0/24`**

Users at Site A reach Host B (`192.168.1.50`) at **`10.10.10.50`**.

### Router A: NAT in both directions

```
interface GigabitEthernet0/1
 description LAN
 ip address 192.168.1.1 255.255.255.0
 ip nat inside
!
interface GigabitEthernet0/0
 description WAN
 ip nat outside
 crypto map VPN-B
!
! Our LAN as seen by Site B
ip nat inside source static network 192.168.1.0 10.5.5.0 /24
! Site B's LAN as seen by us
ip nat outside source static network 192.168.1.0 10.10.10.0 /24
!
! Crypto ACL uses POST-NAT addresses (NAT happens before encryption)
ip access-list extended VPN-TO-B
 permit ip 10.5.5.0 0.0.0.255 192.168.1.0 0.0.0.255
!
crypto map VPN-B 10 ipsec-isakmp
 set peer 198.51.100.2
 set transform-set TS-AES
 match address VPN-TO-B
```

The default route toward the ISP already sends `10.10.10.0/24` out the WAN interface, where
the crypto map lives. If you don't have a default route that way, add a static route for it.

### Router B: no NAT, just the mirror crypto ACL

```
ip access-list extended VPN-TO-A
 permit ip 192.168.1.0 0.0.0.255 10.5.5.0 0.0.0.255
```

!!! warning "Don't forget NAT exemption on Router B"
    If Router B overloads its LAN to the Internet, traffic to `10.5.5.0/24` must be
    **excluded** from that PAT rule. Otherwise it gets PAT'd to the public IP first, no
    longer matches the crypto ACL, and never enters the tunnel.

### Packet walk

| Step | Where | Source | Destination |
|---|---|---|---|
| 1 | Host A sends | `192.168.1.10` | `10.10.10.50` |
| 2 | After NAT on Router A, encrypted | `10.5.5.10` | `192.168.1.50` |
| 3 | Host B replies | `192.168.1.50` | `10.5.5.10` |
| 4 | Router A reverses both NATs | `10.10.10.50` | `192.168.1.10` |

In step 3, Host B sees `10.5.5.10` as an *off-subnet* address, so it sends the reply to its
gateway, and it goes back into the tunnel. That's the whole trick.

## Option 2: PBR (only for partial overlap)

PBR can't help with identical subnets: as shown above, the hosts never send the traffic to
the router. It **does** help when the overlap is a *route* conflict. For example, the remote
side uses a range that you also route somewhere else internally, and you want only specific
users to reach it through the tunnel (route-based VPN with a tunnel interface):

```
ip access-list extended PBR-TO-B
 permit ip 192.168.50.0 0.0.0.255 10.20.5.0 0.0.0.255
!
route-map RM-PBR-VPN permit 10
 match ip address PBR-TO-B
 set interface Tunnel10
!
interface GigabitEthernet0/2
 description Users VLAN 50
 ip policy route-map RM-PBR-VPN
```

## Which one to use

| | NAT | PBR |
|---|---|---|
| Identical subnets on both sides | ✅ Required | ❌ Hosts never reach the router |
| Remote range conflicts with an internal route | Works | ✅ Simpler, no address changes |
| Users must use different IPs or DNS names | Yes (translated addresses) | No |
| Complexity to troubleshoot | Higher | Lower |

## Verify

```
show ip nat translations                   ! both inside and outside static entries present
show crypto isakmp sa                      ! phase 1 up (QM_IDLE)
show crypto ipsec sa peer 198.51.100.2     ! encaps AND decaps counters increasing
show route-map RM-PBR-VPN                  ! (PBR) match counters increasing
```

!!! tip "Test from a real host"
    A ping from the router itself (`ping 10.10.10.50 source 192.168.1.1`) doesn't go
    through inside NAT the same way host traffic does, so it can fail even when the
    design is correct. Test from an actual machine on the LAN.

## Gotchas

- **Static network NAT also hits Internet traffic.** `ip nat inside source static network`
  translates *everything* from the LAN, including Internet-bound traffic, and can break your
  normal PAT. Some IOS versions support a `route-map` on static NAT to limit it to VPN
  traffic. On others, only one-to-one host statics do. Check your platform, or do the
  translation on a firewall or a dedicated device.
- **DNS has to match.** Users at Site A must use the translated `10.10.10.x` addresses.
  Update DNS records (or use DNS doctoring) so names resolve to the virtual addresses.
- **Apps that embed IP addresses** in the payload (SIP, FTP, some legacy apps) may break
  through NAT unless an ALG handles them.
- **Document the translation table.** Six months later, nobody remembers why `10.10.10.50`
  is actually `192.168.1.50`.

## Lessons learned

!!! tip "Takeaways"
    - Identical subnets = **NAT**. Route conflicts = **PBR** may be enough.
    - The crypto ACL always uses **post-NAT** addresses.
    - Check **NAT exemption** on the side that isn't doing the translation.
    - Ask about the remote side's addressing **before** building a tunnel, especially
      with partners and after acquisitions.

<!-- EDIT ME: if you remember how long it took, what the first symptom was, or what you
     tried first, add a short "What I tried first" section here. That's what makes it yours. -->

## Reference

- Cisco: [IPsec Between Two IOS Routers with Overlapping Private Networks](https://www.cisco.com/c/en/us/support/docs/routers/3800-series-integrated-services-routers/107992-IOSRouter-overlapping.html)
