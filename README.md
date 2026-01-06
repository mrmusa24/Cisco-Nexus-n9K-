# Cisco Nexus 9K (n9k-c93240yc-fx2) Initial Setup Guide

This repository contains a complete A-to-Z initial setup for the Cisco Nexus 9K switch (n9k-c93240yc-fx2).  
The guide is designed for Data Center / ISP environment and includes admin account setup, management interface, VLANs, SSH, CoPP, ACL, and basic BGP configuration.

---

## Table of Contents

1. Prerequisites
2. Initial Access
3. System Admin Account Setup
4. Basic System Configuration
5. Management Interface (mgmt0)
6. DNS & NTP
7. SSH & RSA Key
8. Default Interface Layer & Switchport
9. CoPP System Profile
10. Hardware & ACL Tuning
11. VLANs & Interfaces
12. Routed / L3 Ports
13. BGP Configuration
14. Post-Setup Verification
15. Best Practices

---

## Prerequisites

- Cisco Nexus 9K (n9k-c93240yc-fx2) switch  
- Console access or SSH access  
- Basic IP scheme for management network  

---

## Initial Access

# Login to switch via console or SSH
ASN-N9k-Cisco>
enable

---

## System Admin Account Setup

Do you want to enforce secure password standard? y  
Username: admin  
Password: N9k@Admin123

---

## Basic System Configuration

Would you like to enter basic configuration dialog? yes  
Hostname: ASN-N9k-Cisco

---

## Management Interface (mgmt0)

Configure management interface? yes  
IPv4 address: 192.168.10.2/24  
IPv4 gateway: 192.168.10.1  
IPv6: no

---

## DNS & NTP

DNS server: 8.8.8.8  
DNS domain: isp.local  
NTP: no (optional)

---

## SSH & RSA Key

Type of ssh key you would like to generate (dsa/rsa): rsa  
Number of bits: 2048

---

## Default Interface Layer & Switchport

Default interface layer (L3/L2): L2  
Default switchport state (shut/noshut): noshut

---

## CoPP System Profile

CoPP profile (strict/moderate/lenient/dense): strict

---

## Hardware & ACL Tuning

configure terminal  
hardware access-list update atomic  
hardware access-list update default-result permit  
no hardware forwarding layer-2 f1 exclude supervisor  
copy running-config startup-config

---

## VLANs & Interfaces

# VLANs
vlan 10
 name USERS
vlan 20
 name SERVERS

# Access port
interface ethernet1/1
 switchport
 switchport mode access
 switchport access vlan 10
 no shutdown

# Trunk port
interface ethernet1/49
 switchport
 switchport mode trunk
 switchport trunk allowed vlan 10,20
 no shutdown

---

## Routed / L3 Ports

interface ethernet1/50  
 no switchport  
 ip address 10.10.10.1/30  
 no shutdown

---

## BGP Configuration

router bgp 65001  
 bgp log-neighbor-changes  
 neighbor 203.0.113.2 remote-as 65002  
 neighbor 203.0.113.2 description ISP-UPLINK  
 address-family ipv4 unicast  
   network 192.168.10.0/24  
   neighbor 203.0.113.2 activate  
 exit  

---

## Post-Setup Verification

show version  
show running-config  
show interface mgmt0  
show ssh  
show copp status  
show hardware profile  
show bgp summary  

---

## Best Practices

- Use strong passwords and SSH v2 only  
- Enable `hardware access-list update default-result permit` to avoid traffic drop during ACL update  
- CoPP strict profile for CPU protection  
- No shutdown on all uplinks  
- Plan VLANs and port modes before deployment  
- Regularly backup configuration (`copy run start`)  

---

## Notes

- Optional: Configure NTP, SNMP, EVPN VXLAN, and QoS as per environment requirements  
- Use `show running-config | section hardware` to verify hardware / ACL / TCAM settings  

> This guide provides a ready-to-use initial setup for Cisco Nexus 9K (n9k-c93240yc-fx2) for production and lab environments.
