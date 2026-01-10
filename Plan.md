# HomeLab Inventory

This document lists current home lab equipment with clear specs

## Firewall
- Vendor: pfSense appliance (qty: 1)
- Ports:
    - 5 × RJ45 at 2.5 Gbps

## Switches
- MikroTik switches (qty: 2)
    - Features: LAG, MLAG, VLANs, RSTP
    - Ports:
        - 8 × RJ45 at 2.5 Gbps
        - 2 × SFP+ at 10 Gbps

- Smart switch (qty: 1)
    - Features: VLANs, RSTP
    - Dual uplinks to Core-A/Core-B via RSTP (trunk VLANs: 15/20/500)
    - Ports: 16 × RJ45 at 1 Gbps

- PoE switch for video surveillance (managed) (qty: 1)
    - Managed; dual uplinks to Core-A/Core-B via RSTP (access VLAN25 CCVT)
    - Features: PoE, VLANs, RSTP
    - Ports:
        - 16 × RJ45 at 100 Mbps
        - 2 × RJ45 at 1 Gbps
        - 1 × SFP at 1 Gbps

## Wireless Access Points
- MikroTik APs (qty: 2)
    - Features: VLANs
    - Connected to smart switch (trunk VLANs: Home=15, IoT=20, Mgmt=500)
    - Ports: 1 × RJ45 at 1 Gbps

## Servers
- Minisforum MS-01 (qty: 3)
    - CPU: 8 cores / 16 threads
    - Memory: 32 GB DDR5
    - Storage:
        - 1 × 256 GB SSD (OS)
        - 2 × 1 TB SSD (data)
    - Ports:
        - 2 × SFP+ at 10 Gbps
        - 2 × RJ45 at 2.5 Gbps
        - 2 x USB4 Thunderbold 40Gbps

---

## Glossary
- RJ45: Copper Ethernet port (commonly 1/2.5/5/10 Gbps depending on NIC).
- SFP/SFP+: Optical/transceiver slots; SFP typically 1 Gbps, SFP+ 10 Gbps.
- LAG: Link Aggregation Group (IEEE 802.3ad/802.1AX).
- MLAG: Multi-Chassis Link Aggregation (vendor-specific MC-LAG variant).
- VLAN: Virtual LAN (IEEE 802.1Q tagging).

## VLANs
- 10 LAB: servers, containers, and services.
- 15 Home: personal devices (phones, PCs, cast).
- 20 IOT: restricted east-west; allow minimal northbound.
- 25 CCVT: cameras; access only to recorder (container) in LAB.
- 500 Management: device management; no internet egress.

## Addressing
- IPv4 prefixes:
    - VLAN 500 Management: 10.0.0.0/24
    - VLAN 10 LAB: 10.1.0.0/22
    - LAB INTERNAL: 10.255.100.0/24
    - VLAN 15 Home: 172.16.15.0/24
    - VLAN 20 IOT: 172.16.20.0/24
    - VLAN 25 CCVT: 172.16.25.0/24
    - K8S PODS CIRD 10.42.0.0/16
    - K8S Services CIDR 10.43.0.0/16
- IPv6 (ULA base fd7a:6e5b:cafe::/48):
    - VLAN 500 Management: fd7a:6e5b:cafe:500::/64
    - VLAN 10 LAB: fd7a:6e5b:cafe:cafe:10::/64
    - LAB INTERNAL: fd7a:6e5b:beff::/64
    - VLAN 15 Home: fd7a:6e5b:cafe:beef:15::/64
    - VLAN 20 IOT: fd7a:6e5b:cafe:beef:20::/64
    - VLAN 25 CCVT: fd7a:6e5b:cafe:beef:25::/64
    - K8S PODS CIRD fd7a:9999:cafe::/56
    - K8S Services CIDR fd7a:9999:beff::/112


## IPAM
## Management
### IPv4 Addresses
10.0.0.1 Firewall
10.0.0.4 Switch CoreA
10.0.0.5 Switch CoreB
10.0.0.6 Smarth Switch Home
10.0.0.7 AccessPoint_A
10.0.0.8 AccessPoint_B
10.0.0.10 KVM Firewall
10.0.0.11 KVM Baltasar
10.0.0.12 KVM Casper
10.0.0.13 KVM Melchior

### IPv6 Addresses
fd7a:6e5b:cafe:500::1 Firewall
fd7a:6e5b:cafe:500::4 Switch CoreA
fd7a:6e5b:cafe:500::5 Switch CoreB
fd7a:6e5b:cafe:500::6 Smarth Switch Home
fd7a:6e5b:cafe:500::7 AccessPoint_A
fd7a:6e5b:cafe:500::8 AccessPoint_B
fd7a:6e5b:cafe:500::10 KVM Firewall
fd7a:6e5b:cafe:500::11 KVM Baltasar
fd7a:6e5b:cafe:500::12 KVM Casper
fd7a:6e5b:cafe:500::13 KVM Melchior

## Lab
### IPv4 Addresses
10.1.0.1 Firewall
10.1.0.5 K8S VIP
10.1.0.6 Balthasar
10.1.0.7 Casper
10.1.0.8 Melchior

### IPv6 Addresses
fd7a:6e5b:cafe:cafe:10::1 Firewall
fd7a:6e5b:cafe:cafe:10::5 K8S VIP
fd7a:6e5b:cafe:cafe:10::6 Balthasar
fd7a:6e5b:cafe:cafe:10::7 Casper
fd7a:6e5b:cafe:cafe:10::8 Melchior

## Lab internal
### IPv4 Addresses
10.255.100.5 K8S VIP
10.255.100.6 Balthasar
10.255.100.7 Casper
10.255.100.8 Melchior

### IPv6 Addresses
fd7a:6e5b:beff::5 K8S VIP
fd7a:6e5b:beff::6 Balthasar
fd7a:6e5b:beff::7 Casper
fd7a:6e5b:beff::8 Melchior
