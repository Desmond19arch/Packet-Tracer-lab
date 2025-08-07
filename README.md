# 🏨 Hotel Management Network LAB (Packet Tracer Lab)

This LAB simulates a **three-floor hotel network** using Cisco Packet Tracer. It includes complete configuration of VLANs, OSPF routing, DHCP, wireless access, SSH for secure remote management, and port security for device access control.

---

## 📁 Project Overview

A modern hotel network was designed with the following features:

- 📧 Hierarchical network topology across **3 floors**
- 🔐 VLAN segmentation per department
- 🚱 Wireless access for guests and staff
- ⚙️ DHCP server setup for dynamic IP assignment
- 🔄 OSPF routing protocol between floors
- 🔑 SSH for secure remote device access
- 🛡️ Port security enabled on the IT switch

---

## 🏢 Floor and VLAN Layout

| Floor   | Department        | VLAN ID | Subnet         |
| ------- | ----------------- | ------- | -------------- |
| **1st** | Reception         | 80      | 192.168.8.0/24 |
|         | Store             | 70      | 192.168.7.0/24 |
|         | Logistics         | 60      | 192.168.6.0/24 |
| **2nd** | Finance           | 50      | 192.168.5.0/24 |
|         | HR                | 40      | 192.168.4.0/24 |
|         | Sales & Marketing | 30      | 192.168.3.0/24 |
| **3rd** | Admin             | 20      | 192.168.2.0/24 |
|         | IT                | 10      | 192.168.1.0/24 |

---

## 🧰 Devices Used

- 3 Cisco Routers (in IT department/server room)
- 3 Cisco Switches (one per floor)
- Multiple end devices (PCs, phones, printers)
- Wireless Access Points (WAPs) per floor
- Serial DCE cables for router interconnection
- Test-PC for verifying port security

---

## 🔧 Configuration Summary

### Routers and Switches Configuration

```bash
hostname R1
no ip domain-lookup
username admin password admin123

# Enable SSH
ip domain-name hotel.local
crypto key generate rsa
1024
ip ssh version 2
line vty 0 4
 login local
 transport input ssh

# Configure subinterfaces for VLANs
interface Gig0/0.10
 encapsulation dot1Q 10
 ip address 192.168.1.1 255.255.255.0
interface Gig0/0.20
 encapsulation dot1Q 20
 ip address 192.168.2.1 255.255.255.0
interface Gig0/0.30
 encapsulation dot1Q 30
 ip address 192.168.3.1 255.255.255.0
interface Gig0/0.40
 encapsulation dot1Q 40
 ip address 192.168.4.1 255.255.255.0
interface Gig0/0.50
 encapsulation dot1Q 50
 ip address 192.168.5.1 255.255.255.0
interface Gig0/0.60
 encapsulation dot1Q 60
 ip address 192.168.6.1 255.255.255.0
interface Gig0/0.70
 encapsulation dot1Q 70
 ip address 192.168.7.1 255.255.255.0
interface Gig0/0.80
 encapsulation dot1Q 80
 ip address 192.168.8.1 255.255.255.0

# OSPF Configuration
router ospf 1
 network 192.168.0.0 0.0.255.255 area 0
 network 10.10.10.0 0.0.0.3 area 0
 network 10.10.10.4 0.0.0.3 area 0
 network 10.10.10.8 0.0.0.3 area 0
DHCP Configuration (Per VLAN)
bash
Copy
Edit
ip dhcp pool VLAN10
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8

ip dhcp pool VLAN20
 network 192.168.2.0 255.255.255.0
 default-router 192.168.2.1
 dns-server 8.8.8.8

ip dhcp pool VLAN30
 network 192.168.3.0 255.255.255.0
 default-router 192.168.3.1
 dns-server 8.8.8.8

ip dhcp pool VLAN40
 network 192.168.4.0 255.255.255.0
 default-router 192.168.4.1
 dns-server 8.8.8.8

ip dhcp pool VLAN50
 network 192.168.5.0 255.255.255.0
 default-router 192.168.5.1
 dns-server 8.8.8.8

ip dhcp pool VLAN60
 network 192.168.6.0 255.255.255.0
 default-router 192.168.6.1
 dns-server 8.8.8.8

ip dhcp pool VLAN70
 network 192.168.7.0 255.255.255.0
 default-router 192.168.7.1
 dns-server 8.8.8.8

ip dhcp pool VLAN80
 network 192.168.8.0 255.255.255.0
 default-router 192.168.8.1
 dns-server 8.8.8.8
Switch Configuration
bash
Copy
Edit
hostname SW-IT

# VLAN Creation
vlan 10
 name IT
vlan 20
 name Admin
vlan 30
 name Sales
vlan 40
 name HR
vlan 50
 name Finance
vlan 60
 name Logistics
vlan 70
 name Store
vlan 80
 name Reception

# Access Ports
interface range fa0/2 - 5
 switchport mode access
 switchport access vlan 10
interface range fa0/6 - 10
 switchport mode access
 switchport access vlan 20
interface range fa0/11 - 13
 switchport mode access
 switchport access vlan 30
interface range fa0/14 - 16
 switchport mode access
 switchport access vlan 40
interface range fa0/17 - 18
 switchport mode access
 switchport access vlan 50
interface range fa0/19 - 20
 switchport mode access
 switchport access vlan 60
interface range fa0/21 - 22
 switchport mode access
 switchport access vlan 70
interface range fa0/23 - 24
 switchport mode access
 switchport access vlan 80

# Trunk Port
interface fa0/1
 switchport mode trunk
 switchport trunk allowed vlan all

# Port Security (on IT switch)
interface fa0/3
 switchport mode access
 switchport access vlan 10
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation restrict
Wireless Configuration (via WAP GUI)
SSID: Hotel_Wifi_1, VLAN 80 (1st floor)

SSID: Hotel_Wifi_2, VLAN 50 (2nd floor)

SSID: Hotel_Wifi_3, VLAN 10 (3rd floor)

Security: WPA2 with strong passphrases

🖥️ Tools Used
Cisco Packet Tracer

Cisco IOS CLI

🔬 Learning Outcomes
VLAN planning and implementation

Dynamic routing with OSPF

Subnetting and IP address allocation

DHCP and wireless configuration

Remote access via SSH

Switch port security implementation
