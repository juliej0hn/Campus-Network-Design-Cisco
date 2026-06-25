# Campus Network Design — Cisco Packet Tracer

**Course:** CSC230 — Computer Networks  
**Institution:** Misr International University (MIU), Faculty of Computer Science  
**Instructor:** Dr. Mostafa Abdelrahman  

---

## 📄 Project Overview

This project involves designing, building, and fully configuring a multi-site campus network in **Cisco Packet Tracer**. It covers everything from IP addressing and Layer 2 switching to dynamic routing, NAT, IPsec VPN, wireless networking, and network management services.

---

## 📁 File

| File | Description |
|------|-------------|
| `Networks-Final-Project.pka` | Cisco Packet Tracer activity file containing the full network topology and configuration |

> Requires **Cisco Packet Tracer 8.x** or later to open. Download it free from the [Cisco NetAcad](https://www.netacad.com/resources/lab-downloads) portal.

---

## 🗺️ Network Topology

The network consists of three sites interconnected via an ISP:

- **MIU Main Campus** — 4 buildings (Main, S, N, R), each with a Layer 3 multilayer switch (MLS) and VLANs, connected to a central gateway router (`MIU-GW`)
- **MIU Branch 1** — Connected via `Branch-GW` router with two VLANs (VLAN 2 & VLAN 3)
- **Wireless Home Network** — Connected via a wireless home router with laptop, tablet, and smartphone clients

### Key Devices

| Device | Role |
|--------|------|
| MIU-GW | Main campus gateway router |
| Main-MLS, S-MLS, N-MLS, R-MLS | Layer 3 multilayer switches per building |
| Branch-GW | Branch site router (router-on-a-stick) |
| ISP | Internet Service Provider router |
| Wireless Home Router | Home network AP/router |
| DHCP / DNS / Web / Email / NTP+Syslog Servers | Campus services |

---

## 🧩 Project Parts

### Part 1 — VLSM Addressing Scheme (10%)
Designed a VLSM addressing scheme from the base network `172.20.5.0/18`. Subnets were allocated for each VLAN across all buildings and for point-to-point links between MLS devices and `MIU-GW`.

**VLANs configured:**

| Building | VLAN IDs |
|----------|----------|
| Main | VLAN 5, VLAN 15 |
| S | VLAN 30, VLAN 35 |
| N | VLAN 45, VLAN 55 |
| R | VLAN 65, VLAN 75 |

---

### Part 2 — Basic Device Settings & Interface Addressing (10%)
- Configured hostnames, MOTD banners, minimum password length (10 chars)
- Console password: `MIU1234567` | Enable secret: `CSC1234567`
- Disabled domain lookup (`no ip domain-lookup`)
- Encrypted all plaintext passwords
- Configured SSH on all routers
- Applied IP addressing per the Addressing Table on all routers, MLS SVIs, and PCs

---

### Part 3 — Layer 2 Network & Host Support (10%)
- Created VLANs on all switches
- Configured access ports for PC connections
- Configured **802.1Q trunking** between switches
- Configured **Inter-VLAN routing** via SVI interfaces on L3 switches (Main Campus) and router-on-a-stick subinterfaces on `Branch-GW`
- Configured **LACP EtherChannels** between switches as shown in the topology
- Verified with `show vlan brief`, `show interfaces trunk`, `show etherchannel summary`

---

### Part 4 — Routing Protocols (10%)

**OSPFv2 (Process ID 100)** — MIU-GW, Main-MLS, S-MLS:
- Configured router IDs
- Advertised networks, set passive interfaces on LAN ports
- Propagated default route via OSPF

**EIGRP (AS 10)** — MIU-GW, N-MLS, R-MLS:
- Advertised directly connected networks
- Set passive interfaces on LAN ports
- Propagated default route via EIGRP

**Route Redistribution** — two-way redistribution between OSPF and EIGRP on `MIU-GW`

**Static & Default Routes:**
- `MIU-GW` and `Branch-GW` → default route pointing to ISP
- `ISP` → static routes to MIU-GW, Branch-GW, and the Home Network

---

### Part 5 — DHCP Server (5%)
- **Main Campus DHCP Server:** Pools for VLAN 5, 15, 30, 35 — first 3 usable addresses excluded; MLS devices configured as DHCP relay agents (`ip helper-address`)
- **Branch-GW as DHCP Server:** Pools `B-LAN2` (VLAN 2) and `B-LAN3` (VLAN 3) — first 5 usable addresses excluded
- PC1–PC6 and PC11–PC12 configured to obtain addresses via DHCP

---

### Part 6 — NAT (5%)
- **Dynamic NAT with PAT** on `MIU-GW` using the 3rd address from `209.165.200.224/28`
- **Static NAT** mapping the `miu.edu.eg` web server's inside address to the 4th address from `209.165.200.224/28`
- Inside/outside NAT interfaces configured
- Verified with `show ip nat translations`

---

### Part 7 — Network Management Features (10%)

| Feature | Details |
|---------|---------|
| NTP | All devices sync to the NTP/Syslog server |
| Syslog | All devices send logs to the Syslog server |
| DNS | `miu.edu.eg` → Web Server IP; `email.miu.edu.eg` → Email Server IP |
| Email Server | SMTP & POP3 enabled; domain `email.miu.edu.eg`; users: `user1/cs1`, `user2/cs2` |
| Web Server | Hosts MIU homepage at `miu.edu.eg` with MIU logo |

---

### Part 8 — Site-to-Site IPsec VPN (10%)
- Configured a site-to-site IPsec VPN tunnel between `MIU-GW` and `Branch-GW`
- Verified encrypted traffic between the main campus and the branch site

---

### Part 9 — Wireless Home Router (10%)
- **SSID:** `MIU-CSC230` (2.4 GHz only)
- **Security:** WPA2 Personal, AES encryption
- **Passphrase:** `miu_csc230`
- Connected laptop, tablet, and smartphone wirelessly; verified connectivity

---

### Part 10 — Connectivity Verification (10%)
- Ping tests between all devices across all sites
- Verified IP addressing with `show ip interface brief` on all devices
- Verified routing tables with `show ip route` on all routers


---

---

## 🛠️ Technologies & Protocols

| Category | Details |
|----------|---------|
| Addressing | VLSM, IPv4 |
| Layer 2 | VLANs, 802.1Q Trunking, STP, LACP EtherChannel |
| Routing | OSPFv2, EIGRP, Route Redistribution, Static/Default Routes |
| Services | DHCP, DNS, NTP, Syslog, HTTP, SMTP/POP3 |
| Security | NAT/PAT, Static NAT, Site-to-Site IPsec VPN, SSH, Password Encryption |
| Wireless | WPA2-Personal, AES, 2.4 GHz |

---

## 🚀 Getting Started

1. Install [Cisco Packet Tracer](https://www.netacad.com/resources/lab-downloads) (free with a NetAcad account).
2. Clone or download this repository:
   ```bash
   git clone https://github.com/julijek0hn/Campus-Network-Design-Cisco.git
   ```
3. Open `Networks-Final-Project.pka` in Packet Tracer.
4. Use **Simulation mode** to trace packet flow between devices.
5. Click any device → **CLI tab** to inspect running configurations.

---


---
