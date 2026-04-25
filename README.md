# 🌐 Enterprise-Grade Multi-Site VPN & Infrastructure Lab

## 📝 Project Overview
This lab simulates a secure corporate environment connecting a **Remote Branch** to a **Corporate HQ** through a public ISP. The goal was to centralize network services (DHCP/DNS) while maintaining strict department segmentation.

## 🛠️ Technical Stack
* **Networking:** Cisco IOS (Routers & Switches).
* **Security:** Site-to-Site IPsec VPN (AES-256), Standard & Extended ACLs.
* **Services:** DHCP Relay (IP Helper), DNS (A-Records).
* **VLANs:** 802.1Q Trunking, Inter-VLAN Routing (Router-on-a-Stick).

## 📐 Topology
![Network Topology](images/)

## 💡 Design Philosophy & Decisions

### Why this design?
The architecture was chosen to mirror a mid-sized enterprise seeking a balance between security and cost-efficiency. By utilizing a Site-to-Site VPN over a simulated public ISP, the business avoids expensive dedicated lines while maintaining a high level of encryption (AES-256) for sensitive data transit.

### Why use DHCP instead of Static?
While static IPs are manageable in very small environments, this design utilizes a **DHCP Relay (IP Helper)** for several critical reasons:
* **Centralized Management:** All IP scopes are managed on a single HQ server, reducing the administrative overhead of configuring local pools on multiple branch routers.
* **Scalability:** The branch can easily grow from a few workstations to a full department without requiring manual IP assignments or local router modifications.
* **Reduced Human Error:** Automating address assignment prevents IP conflicts and documentation mismatches that commonly occur with manual static entries.

### Were there other approaches?
* **Dynamic Routing:** I could have used OSPF or EIGRP to manage routes. However, for a simple two-site setup, **Static Routing** was chosen to minimize CPU overhead and maintain tighter control over path selection.
* **Layer 3 Switching:** Instead of Router-on-a-Stick, a Layer 3 switch could perform inter-VLAN routing at hardware speeds. This was considered for the HQ, but the current design focuses on demonstrating fundamental sub-interface logic common in branch deployments.
* **Remote Access VPN:** I could have configured a client-to-site VPN. I chose Site-to-Site because it provides an "always-on" connection for the entire office infrastructure rather than relying on individual software clients.

## 🚀 Key Configurations & Achievements

### 1. Secure IPsec VPN Tunnel
Established Phase 1 (ISAKMP) and Phase 2 (IPsec) tunnels using **AES-256 encryption**. This allows the Branch Office to access HQ resources over an "unsafe" simulated internet connection.

### 2. Segmented Inter-VLAN Routing
Configured a **Router-on-a-Stick** topology to allow controlled communication between departments while significantly reducing broadcast traffic.

### 3. Centralized Identity & Addressing
Implemented a **DHCP Relay Agent** on the Branch router. This enables remote clients to pull dynamic IP addresses from the HQ Server (`192.168.10.5`) across the VPN tunnel.

## 🔍 Troubleshooting Log
* **DNS Resolution Failure:** Identified that the DNS Server lacked a Default Gateway, preventing return traffic from reaching the Branch.
* **The "Black Hole" Port:** Discovered a server was assigned to the wrong VLAN; corrected by reassigning Switchport `Fa0/10` to VLAN 10.
* **ACL Optimization:** Modified VPN Access Control Lists to permit UDP/DNS traffic that was initially being dropped by restrictive permit statements.

## 📖 How to Use
1. Download [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer).
2. Download the `.pkt` file from this repository.
3. Open the file to inspect the CLI configurations for each device.
