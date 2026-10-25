# Field Stories

Real incidents from production networks: what broke, how I found it,
and what I learned fixing it.

## Stories

- [Overlapping Subnets Across an IPsec Tunnel](overlapping-subnets-ipsec.md): same /24 on both sides of a VPN, solved with NAT (and when PBR is enough)
- [Pre-Staging FortiGate VIPs Took Down the Old Firewall's NATs](fortigate-vip-proxy-arp.md): how a default ARP setting caused an outage before the cutover even started
- [Safe Changes on Remote Switches: Arm the Safety Net First](safe-remote-changes.md): `reload in`, the missing `add` keyword, and planning a conservative change window
- [Taking a Bad ISP Out of BGP Without Shutting the Session](bgp-drain-isp.md): a deny-all route-map in both directions, soft resets, and why restoring takes minutes
- [One Port Pulled From a Port-Channel Took the Whole Uplink Down](errdisable-port-channel.md): EtherChannel misconfig guard, finding err-disabled ports, and errdisable recovery
- [SFPs Explained: Types, Reach, and How to Check Compatibility](sfp-types-compatibility.md): choosing the right optic, and using Cisco, Fortinet and Palo Alto compatibility resources before you order
- [Adding Meraki Devices: Claim the Order, Not the Serial Numbers](meraki-claim-by-order.md): why one order claim beats typing serials, and the trap that blocks it
- [Reading the Meraki Route Table: Why One Site Shows the Whole Organization](meraki-route-table.md): Auto VPN routes, telling local subnets from remote ones, and the routing-priority surprise
- [Fiber Link Won't Come Up? Check the Polarity](fiber-polarity.md): reading Tx/Rx light levels, spotting swapped strands, and fixing it at one end
- [Hitting the Route Limit on an Old Switch: TCAM and SDM Templates](switch-max-routes.md): why routes beyond the hardware limit get CPU-switched, and how to check and fix it
- [Life of a Packet: DHCP, Step by Step](dhcp-life-of-packet.md): DORA through a relay, renewals and APIPA, where DHCP breaks, and the commands to follow the packet
- [Life of a Packet: DNS, Step by Step](dns-life-of-packet.md): stub and recursive resolvers, root to authoritative, caching and TTLs, where DNS breaks, and the commands to trace it
- [Ping and nslookup Disagree? The Hosts File, and a Bad Cloud Edge](hosts-file-troubleshooting.md): how Windows resolves names, using the hosts file to steer around a bad edge location, and the traps to avoid
- [Racking Gear: Rack Sizes, Cage Nuts, and PDU vs. UPS](rack-power-basics.md): rack units and cabinet sizes, why to carry spare cage nuts, and feeding dual power supplies from A and B
- [802.1X Rollout Surprises: Port Bounces and Docking Stations](dot1x-bounce-docks.md): why connected devices need a port bounce, and how MAC pass-through fixes docks
- [Fiber Basics on Site: Color Codes, Splicing, Patch Panels and Labels](fiber-colors-labeling.md): single-mode vs. multimode at a glance, the 12-color strand order, and labeling both ends
- [Tracking Down a Machine, and Why Moving Cables Seems Not to Work](tracking-a-machine.md): IP to MAC to port with ARP, MAC tables and CDP/LLDP, and the cache timers behind confusing cable moves
- [Keeping It Cool: Hot and Cold Aisles, Small Data Rooms, and Fan Direction](cooling-basics.md): inlet temperatures, hot and cold aisles, cooling options for small rooms, and matching switch airflow to the rack
- [The Virtual Firewall That Locked Us Out of Its Own Console](virtual-firewall-lockout.md): VMware management behind a virtual firewall, the circular dependency it creates, and how to break it before a change
- [Nexus Basics: Enable the Feature First (and Be Careful Turning It Off)](nexus-feature-command.md): why commands "don't exist" on a new Nexus, the common features, and why no feature deletes config
- [Reading show interface and show policy-map interface](show-interface-policy-map.md): which counters mean a physical problem, which mean congestion, and where QoS is dropping traffic
- [How I Troubleshoot: A Method, and Reading pathping and WinMTR Properly](troubleshooting-method-pathping.md): a structured loop, five ways to narrow it down, and where packet loss really starts
- [Catalyst Center Templates: Using Variables to Assign VLANs and Subnets](catalyst-center-templates.md): one template for every site, deriving VLANs and subnets from a site number, interface loops, and safe rollouts
- [FortiGate Dropping Asymmetric Traffic: Reverse Path Check Fail](fortigate-asymmetric-routing.md): finding RPF drops in a debug flow, and the fixes from symmetric routing to a same-distance, higher-priority route
- [Palo Alto Stuck Sessions: When an Old Session Won't Let Go](palo-alto-stuck-sessions.md): why long-lived flows keep the old path or NAT after a change, and how to find and clear them
- [FlexConnect APs: Why the Native VLAN Matters](flexconnect-native-vlan.md): trunk ports for FlexConnect, matching the native VLAN to the flex profile, and the mistakes that stop APs joining
- [The New AP That Looked Like an Attacker](rogue-ap-production-ssid.md): an unmanaged AP broadcasting the production SSID, how rogue detection works, and provisioning APs safely
- [When an Old Switch Runs Out of Spanning-Tree Instances](stp-instance-limit.md): PVST+ instance limits, why VLANs beyond them are unprotected, and pruning or MST as the fix
- [Temporary Fixes That Quietly Become Permanent](temporary-fixes.md): why workarounds stick, the normalization of deviance, and a four-step process to close them
