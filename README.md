inter-vlan-routing-lab
Lab for VLANs, trunking, DHCP and inter-VLAN routing in Cisco Packet Tracer.
......................................................................................................
🌐 VLANs, Trunking, DHCP, and Inter-VLAN Routing Lab

📋 Overview
This lab demonstrates how to set up two VLANs with DHCP, trunking between two switches, and inter-VLAN routing using a router-on-a-stick in Cisco Packet Tracer.

---

📝 Objectives
✅ Create two VLANs:
- VLAN 10 → IT → `192.168.1.0/24`
- VLAN 20 → HR → `192.168.2.0/24`

✅ Configure two switches with the VLANs.  
✅ Connect the switches via a trunk link.  
✅ Configure a router-on-a-stick to route between VLANs.  
✅ Provide DHCP for each VLAN from the router.

---

🧰 Topology
- 1 Router (using `GigabitEthernet 0/0`)
- 2 Switches
- 4 PCs:
  - PC1 & PC2 → IT VLAN
  - PC3 & PC4 → HR VLAN

 Connections:
- Router `g0/0` ↔ Switch1 `f0/5` (Trunk)
- Switch1 `f0/24` ↔ Switch2 `f0/24` (Trunk)
- PCs connected to appropriate VLAN access ports

---

🚀 Configuration Steps

🔷 VLANs on Switch1
```bash
enable
configure terminal

vlan 10
name IT
exit

vlan 20
name HR
exit

interface range fa0/1 - 2
switchport mode access
switchport access vlan 10
exit

interface range fa0/3 - 4
switchport mode access
switchport access vlan 20
exit
🔷 VLANs on Switch2
Repeat the same configuration as Switch1.

🔷 Trunk Between Switches
bash
Copy
Edit
interface fa0/24
switchport mode trunk
exit
(On both Switch1 and Switch2)

🔷 Trunk Between Router and Switch1
bash
Copy
Edit
interface fa0/5
switchport mode trunk
exit
🔷 Router-on-a-Stick
bash
Copy
Edit
enable
configure terminal

interface g0/0
no shutdown
exit

interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.1.1 255.255.255.0
exit

interface g0/0.20
encapsulation dot1Q 20
ip address 192.168.2.1 255.255.255.0
exit
🔷 DHCP on Router
bash
Copy
Edit
ip dhcp excluded-address 192.168.1.1 192.168.1.10
ip dhcp excluded-address 192.168.2.1 192.168.2.10

ip dhcp pool IT
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1
exit

ip dhcp pool HR
network 192.168.2.0 255.255.255.0
default-router 192.168.2.1
exit
🔗 Testing
✅ PCs in IT VLAN should get IPs from 192.168.1.0/24.
✅ PCs in HR VLAN should get IPs from 192.168.2.0/24.
✅ Ping between PCs across VLANs should work (inter-VLAN routing).

🛠️ Useful Commands
show vlan brief → check VLANs on switch

show ip dhcp binding → check DHCP leases on router

ping → test connectivity
