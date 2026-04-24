# Multi-Site Enterprise Network & VPN Infrastructure

## 🌐 Project Overview
This lab simulates a secure corporate environment connecting a **Remote Branch** to a **Corporate HQ** through a public ISP. The goal was to centralize network services (DHCP/DNS) while maintaining strict department segmentation.

## 🛠️ Technical Stack
* **Networking:** Cisco IOS (Routers & Switches)
* **Security:** IPsec VPN (Site-to-Site), Standard & Extended ACLs
* **Services:** DHCP Relay (IP Helper), DNS (A-Records)
* **VLANs:** 802.1Q Trunking, Inter-VLAN Routing

## 📐 Topology
![Network Topology](images/topology.png)

## 🚀 Key Configurations & Achievements

### 1. Secure IPsec VPN Tunnel
Established a Phase 1 (ISAKMP) and Phase 2 (IPsec) tunnel using AES-256 encryption. This allows the Branch Office to access HQ resources over a simulated "unsafe" internet connection.

### 2. Segmented Inter-VLAN Routing
* **VLAN 10:** Servers & HR Dept
* **VLAN 20:** IT Dept
Configured a **Router-on-a-Stick** topology to allow controlled communication between departments while reducing broadcast traffic.

### 3. Centralized Identity & Addressing
Implemented a **DHCP Relay Agent** on the Branch router. This allows remote clients to pull dynamic IP addresses from the HQ Server (`192.168.10.5`) across the VPN.

## 🔍 Troubleshooting Log
* **The "Black Hole" Port:** Identified that the DNS Server was in the wrong VLAN. Corrected by reassigning Switchport `Fa0/10` to VLAN 10.
* **Gateway Recovery:** Resolved a "Connection Reset" error by configuring the missing Default Gateway on the HQ Servers, allowing return traffic to reach the Branch.
* **ACL Optimization:** Modified VPN ACLs to allow UDP/DNS traffic which was initially being dropped by restrictive permit statements.

## 📖 How to Use
1. Download [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer).
2. Download the `.pkt` file from this repository.
3. Open the file to inspect the CLI configurations for each device.
