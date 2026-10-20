# Enterprise VLAN Segmentation & Inter-VLAN Routing

## Project Overview

This project demonstrates the design, configuration, and validation of a segmented enterprise network using Cisco Packet Tracer.

The network uses multiple VLANs to separate departments and guest devices, a multilayer switch for inter-VLAN routing, 802.1Q trunk links, DHCP address pools, and an extended access control list (ACL) intended to restrict guest access to internal networks.

**Project type:** Enterprise Networking Lab  
**Platform:** Cisco Packet Tracer  
**Technologies:** VLANs, 802.1Q, Layer 3 Switching, Inter-VLAN Routing, DHCP, ACLs, Static Routing, Spanning Tree Protocol

## Objectives

- Design a logical enterprise network with departmental VLANs.
- Separate IT, Administration, HR, Guest, and Management traffic.
- Configure VLAN access ports and trunk links.
- Enable inter-VLAN routing on a multilayer switch.
- Configure DHCP pools for client networks.
- Implement a guest isolation policy using an extended ACL.
- Configure a default route toward an upstream network.
- Validate connectivity and document outstanding issues.

## Network Segmentation

| VLAN ID | Name | Network | Default Gateway |
|---:|---|---|---|
| 10 | IT | 192.168.10.0/24 | 192.168.10.1 |
| 20 | ADMINISTRATION | 192.168.20.0/24 | 192.168.20.1 |
| 30 | HR | 192.168.30.0/24 | 192.168.30.1 |
| 40 | GUEST | 192.168.40.0/24 | 192.168.40.1 |
| 99 | MANAGEMENT | 192.168.99.0/24 | 192.168.99.1 |

The first twenty addresses in each DHCP client subnet are excluded from dynamic allocation. DHCP pools are configured for VLANs 10, 20, 30, and 40.

## Network Architecture

SW1 is the central multilayer switch. It provides switched virtual interfaces (SVIs), inter-VLAN routing, DHCP services, and a default route to the upstream network.

SW3 provides access connectivity for departmental and guest clients. Its access ports assign clients to the appropriate VLAN, while its trunk uplink carries VLAN traffic toward SW1.

### Main Components

- **SW1:** Multilayer switch, SVIs, inter-VLAN routing, DHCP, and ACL configuration.
- **SW3:** Access switch with VLAN 30 and VLAN 40 client ports.
- **Guest PC:** Client assigned to VLAN 40.
- **Departmental clients:** IT, Administration, and HR endpoints.
- **Upstream connection:** SW1 GigabitEthernet0/1, configured with `10.0.0.2/30`; next hop `10.0.0.1`.

## Key Configurations

### VLANs and Trunking

The network uses VLANs 10, 20, 30, 40, and 99. Trunk links use IEEE 802.1Q and are configured to allow these VLANs.

### Inter-VLAN Routing

IP routing is enabled on SW1. Each departmental VLAN has an SVI serving as its default gateway.

### DHCP

DHCP pools provide network settings for VLANs 10, 20, 30, and 40, including the default gateway and DNS server.

### Guest Isolation

An extended ACL named `GUEST-ISOLATION` is configured to deny traffic from the Guest subnet to the IT, Administration, HR, and Management subnets, while permitting other IP traffic.

**Validation note:** The ACL appears in the running configuration, but the switch's `show ip interface vlan 40` output reports that no inbound ACL is applied. Therefore, successful enforcement of the isolation policy has not been confirmed.

## Validation Results

| Test | Result |
|---|---|
| VLAN creation and status | Verified |
| SW3 access-port assignment | Verified |
| 802.1Q trunk status | Verified |
| SVI status | All five SVIs reported up/up |
| Connected routes on SW1 | Verified |
| Default route via 10.0.0.1 | Verified |
| Guest PC to 192.168.40.1 | Successful |
| IT PC to 192.168.10.1 | Successful |
| DHCP for VLANs 10, 20, and 30 | Leases observed |
| DHCP for VLAN 40 | Not yet verified |
| Guest isolation ACL enforcement | Not yet verified |

## Known Limitations

1. The Guest PC currently uses the static address `192.168.40.50/24`.
2. DHCP operation for VLAN 40 requires further validation.
3. The Guest isolation ACL is configured, but its application to VLAN 40 has not been confirmed.
4. Failed pings from Guest to other departmental PCs are not sufficient on their own to prove ACL enforcement, because the destination host's reachability must also be established.

## Skills Demonstrated

- Cisco switching and VLAN segmentation
- 802.1Q trunk configuration
- Layer 3 switching and inter-VLAN routing
- DHCP configuration and troubleshooting
- Extended ACL configuration
- IP addressing and subnetting
- Connectivity troubleshooting using Cisco IOS commands
- Network documentation and validation

## Future Improvements

- Verify and correct ACL application.
- Validate DHCP allocation for every VLAN.
- Test allowed and denied traffic with known reachable endpoints.
- Add SSH management access and secure device credentials.
- Add a dedicated management access policy for VLAN 99.
- Expand the lab with redundant links and dynamic routing.

## Project Status

**Core network configuration implemented; final Guest DHCP and ACL validation pending.**

This lab is intended for educational and portfolio purposes and is not a production-ready network configuration.
