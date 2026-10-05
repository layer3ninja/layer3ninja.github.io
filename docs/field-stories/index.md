# Field Stories

Real incidents from production networks: what broke, how I found it,
and what I learned fixing it.

## Stories

- [Overlapping Subnets Across an IPsec Tunnel](overlapping-subnets-ipsec.md): same /24 on both sides of a VPN, solved with NAT (and when PBR is enough)
- [Pre-Staging FortiGate VIPs Took Down the Old Firewall's NATs](fortigate-vip-proxy-arp.md): how a default ARP setting caused an outage before the cutover even started
- [Safe Changes on Remote Switches: Arm the Safety Net First](safe-remote-changes.md): `reload in`, the missing `add` keyword, and planning a conservative change window
- [Taking a Bad ISP Out of BGP Without Shutting the Session](bgp-drain-isp.md): a deny-all route-map in both directions, soft resets, and why restoring takes minutes
- [One Port Pulled From a Port-Channel Took the Whole Uplink Down](errdisable-port-channel.md): EtherChannel misconfig guard, finding err-disabled ports in one command, and errdisable recovery
