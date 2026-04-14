# Network-Topology
3-floor enterprise network topology using Cisco Packet Tracer with VLANs, HSRP, OSPF, DHCP relay, DNS, and port security. Includes core, distribution, and access layer design with redundancy and inter-VLAN routing.

# Campus Network Project Documentation
1. Project Overview:

This project is a campus network topology built in Cisco Packet Tracer. It includes:

Access layer switches
Multilayer switches
Core routers
VLAN segmentation
VTP
Trunking
Inter-VLAN routing
HSRP
OSPF
DHCP relay
DNS and DHCP servers
Basic port security

The goal of this project is to create a scalable and redundant enterprise-style campus network.

# 2. VLAN and IP Address Plan:
   
VLAN ID	Department / Use	Network
10	Printer	    10.1.1.0/24
20	AP	          10.1.2.0/24
30	Sales	       10.1.3.0/24
40	HR	          10.1.4.0/24
50	Finance	    10.1.5.0/24
60	Admin	       10.1.6.0/24
70	ICT	       10.1.7.0/24
80	Server Room	 10.1.8.0/24
99	Blackhole / Unused Ports	Reserved for security

# 3. Basic Configuration Done on All Switches:

The following basic configuration was applied on all Layer 2 switches and multilayer switches.

enable
configure terminal
hostname <SWITCH-NAME>
banner motd #No Authorised Access!!!#
no ip domain lookup

line console 0
password cisco
login
exit

enable password cisco123
service password-encryption
do write memory

Purpose:
- hostname gives a unique name to the device
- banner motd displays a warning message
- no ip domain lookup prevents DNS lookup delay for wrong commands
- console password protects local access
- enable password protects privileged mode
- service password-encryption encrypts plain text passwords

# 4. Access Layer Switch Configuration:

Each access switch was configured for its own department VLAN. These switches work in VTP client mode and connect to both multilayer switches using trunk ports. Based on your notes, access switches use VTP client mode and trunk uplinks on fa0/4-5.

# 4.1 SALES-SW:
Purpose:

This switch connects Sales department devices.

Configuration Done
enable
configure terminal
hostname SALES-SW
banner motd #No Authorised Access!!!#
no ip domain lookup

line console 0
password cisco
login
exit

enable password cisco123
service password-encryption

vtp domain Campus-NW
vtp mode client
vtp password Witnt@123
vtp version 2
Port Configuration
interface range fa0/6-24
switchport mode access
switchport access vlan 30
exit

interface range fa0/2    # Access point port configured on vlan 20. -- same to all SWs.
switchport mode access
switchport access vlan 20
exit

interface range fa0/3     # printer port configured for Vlan 10. -- Same to all SWs.
switchport mode access
switchport access vlan 10
exit

interface range fa0/4-5
switchport mode trunk
exit

Explanation:
fa0/6-24 are used for Sales end devices
VLAN 30 is assigned to Sales users
fa0/4-5 are uplinks to multilayer switches
trunk is used because multiple VLANs travel between access and distribution layers

# 4.2 HR-SW:
Purpose:

This switch connects HR department devices.

Configuration Done
enable
configure terminal
hostname HR-SW
banner motd #No Authorised Access!!!#
no ip domain lookup

line console 0
password cisco
login
exit

enable password cisco123
service password-encryption

vtp domain Campus-NW
vtp mode client
vtp password Witnt@123
vtp version 2
Port Configuration
interface range fa0/6-24
switchport mode access
switchport access vlan 40
exit

interface range fa0/2    # Access point port configured on vlan 20. -- same to all SWs.
switchport mode access
switchport access vlan 20
exit

interface range fa0/3     # printer port configured for Vlan 10. -- Same to all SWs.
switchport mode access
switchport access vlan 10
exit

interface range fa0/4-5
switchport mode trunk
exit

Explanation:
VLAN 40 is used for HR department
access ports connect HR users
trunk ports connect this switch to multilayer switches.

# 4.3 FIN-SW:
Purpose:

This switch connects Finance department devices.

Configuration Done
enable
configure terminal
hostname FIN-SW
banner motd #No Authorised Access!!!#
no ip domain lookup

line console 0
password cisco
login
exit

enable password cisco123
service password-encryption

vtp domain Campus-NW
vtp mode client
vtp password Witnt@123
vtp version 2
Port Configuration
interface range fa0/6-24
switchport mode access
switchport access vlan 50
exit

interface range fa0/2    # Access point port configured on vlan 20. -- same to all SWs.
switchport mode access
switchport access vlan 20
exit

interface range fa0/3     # printer port configured for Vlan 10. -- Same to all SWs.
switchport mode access
switchport access vlan 10
exit

interface range fa0/4-5
switchport mode trunk
exit

Port SecurityConfigure done at the Finance- SW:

interface range fa0/6-24
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation shutdown
exit
do write memory

Explanation:
VLAN 50 is used for Finance
port security allows only one learned MAC address
sticky MAC saves the connected device MAC automatically
violation mode shutdown disables the port if an unauthorized device connects.

# 4.4 ADMIN-SW:
Purpose:

This switch connects Admin department devices.

Configuration Done
enable
configure terminal
hostname ADMIN-SW
banner motd #No Authorised Access!!!#
no ip domain lookup

line console 0
password cisco
login
exit

enable password cisco123
service password-encryption

vtp domain Campus-NW
vtp mode client
vtp password Witnt@123
vtp version 2
Port Configuration
interface range fa0/6-24
switchport mode access
switchport access vlan 60
exit

interface range fa0/2    # Access point port configured on vlan 20. -- same to all SWs.
switchport mode access
switchport access vlan 20
exit

interface range fa0/3     # printer port configured for Vlan 10. -- Same to all SWs.
switchport mode access
switchport access vlan 10
exit

interface range fa0/4-5
switchport mode trunk
exit
Explanation:
VLAN 60 is used for Admin users
access ports connect department devices
trunk ports carry VLAN traffic to multilayer switches.

# 4.5 ICT-SW:
Purpose:

This switch connects ICT department devices.

Configuration Done
enable
configure terminal
hostname ICT-SW
banner motd #No Authorised Access!!!#
no ip domain lookup

line console 0
password cisco
login
exit

enable password cisco123
service password-encryption

vtp domain Campus-NW
vtp mode client
vtp password Witnt@123
vtp version 2
Port Configuration
interface range fa0/6-24
switchport mode access
switchport access vlan 70
exit

interface range fa0/2    # Access point port configured on vlan 20. -- same to all SWs.
switchport mode access
switchport access vlan 20
exit

interface range fa0/3     # printer port configured for Vlan 10. -- Same to all SWs.
switchport mode access
switchport access vlan 10
exit

interface range fa0/4-5
switchport mode trunk
exit

Explanation:
VLAN 70 is used for ICT users
end devices connect through access ports
uplinks to multilayer switches are trunks.

# 4.6 SERVER-SW:
Purpose:

This switch connects server room devices such as DHCP, DNS, and management system.

Configuration Done
enable
configure terminal
hostname SERVER-SW
banner motd #No Authorised Access!!!#
no ip domain lookup

line console 0
password cisco
login
exit

enable password cisco123
service password-encryption

vtp domain Campus-NW
vtp mode client
vtp password Witnt@123
vtp version 2
Port Configuration
interface range fa0/1-3 & 6-24
switchport mode access
switchport access vlan 80
exit

interface range fa0/4-5
switchport mode trunk
exit

Explanation:
VLAN 80 is used for the server room
servers are connected in this VLAN
server room devices use static IP addresses

# 5. Unused Port Security / Blackhole VLAN:

The unused ports were placed in VLAN 99 and shut down for security.

# This configuration done at all the switches.
vlan 99
name BLACKHOLE

interface range gig0/1-2
switchport mode access
switchport access vlan 99
shutdown

Explanation:
unused ports are moved to VLAN 99
ports are administratively shut down
this improves switch security and reduces unauthorized access.

# 6. Multilayer Switch Configuration:

The multilayer switches were used for:

VTP server role
trunk aggregation from access layer
inter-VLAN routing
HSRP default gateway redundancy
DHCP relay
routing toward core routers

Both of the multilayer switches were configured in VTP server mode, with HSRP and SVIs for VLANs 10–80.

# 6.1 MLT-SW1:
Purpose

Primary multilayer switch for inter-VLAN routing and active HSRP gateway.

Basic and Management Configuration
enable
configure terminal
hostname MLT-SW1
banner motd #No Authorised Access!!!#
no ip domain lookup

line console 0
password cisco
login
exit

enable password cisco123
service password-encryption

SSH Configuration:

configure terminal
ip domain name cisco.net
username admin password cisco
crypto key generate rsa

line vty 0 15
login local
transport input ssh
exit
do write memory

VTP Configuration:

configure terminal
vtp domain Campus-NW
vtp mode server
vtp password Witnt@123
vtp version 2
Trunk Ports to Access Layer
interface range gig1/0/1-6
switchport mode trunk

Routed Uplinks to Core Routers:

/30 links for MLT-SW1: 172.16.0.0/30 and 172.16.0.4/30.

interface gig1/0/7
no switchport
ip address 172.16.0.1 255.255.255.252
exit

interface gig1/0/8
no switchport
ip address 172.16.0.5 255.255.255.252
exit

Enable Layer 3 Routing:
ip routing

# SVI and HSRP Configuration:
interface vlan 10
ip address 10.1.1.2 255.255.255.0
standby 10 ip 10.1.1.1
standby 10 priority 110
standby 10 preempt
no shutdown
exit

interface vlan 20
ip address 10.1.2.2 255.255.255.0
standby 20 ip 10.1.2.1
standby 20 priority 110
standby 20 preempt
no shutdown
exit

interface vlan 30
ip address 10.1.3.2 255.255.255.0
standby 30 ip 10.1.3.1
standby 30 priority 110
standby 30 preempt
no shutdown
exit

interface vlan 40
ip address 10.1.4.2 255.255.255.0
standby 40 ip 10.1.4.1
standby 40 priority 110
standby 40 preempt
no shutdown
exit

interface vlan 50
ip address 10.1.5.2 255.255.255.0
standby 50 ip 10.1.5.1
standby 50 priority 110
standby 50 preempt
no shutdown
exit

interface vlan 60
ip address 10.1.6.2 255.255.255.0
standby 60 ip 10.1.6.1
standby 60 priority 110
standby 60 preempt
no shutdown
exit

interface vlan 70
ip address 10.1.7.2 255.255.255.0
standby 70 ip 10.1.7.1
standby 70 priority 110
standby 70 preempt
no shutdown
exit

interface vlan 80
ip address 10.1.8.2 255.255.255.0
standby 80 ip 10.1.8.1
standby 80 priority 110
standby 80 preempt
no shutdown
exit

# DHCP Relay: --It forwards DHCP between Client & DHCP server.
interface vlan 10
ip helper-address 10.1.8.10
exit

interface vlan 20
ip helper-address 10.1.8.10
exit

interface vlan 30
ip helper-address 10.1.8.10
exit

interface vlan 40
ip helper-address 10.1.8.10
exit

interface vlan 50
ip helper-address 10.1.8.10
exit

interface vlan 60
ip helper-address 10.1.8.10
exit

interface vlan 70
ip helper-address 10.1.8.10
exit

interface vlan 80
ip helper-address 10.1.8.10
exit

# 6.2 MLT-SW2
Purpose

Backup multilayer switch for inter-VLAN routing and standby HSRP gateway.

Basic and Management Configuration
enable
configure terminal
hostname MLT-SW2
banner motd #No Authorised Access!!!#
no ip domain lookup

line console 0
password cisco
login
exit

enable password cisco123
service password-encryption
SSH Configuration:

configure terminal
ip domain name cisco.net
username admin password cisco
crypto key generate rsa
1024
line vty 0 15
login local
transport input ssh
exit
ip ssh version 2
do write memory

VTP Configuration:

configure terminal
vtp domain Campus-NW
vtp mode Client      # Packet tracer does not support VTP Ver3.
vtp password Witnt@123
vtp version 2
Trunk Ports
interface range gig1/0/1-6
switchport mode trunk

Routed Uplinks to Core Routers

/30 links for MLT-SW2: 172.16.0.8/30 and 172.16.0.12/30.

interface gig1/0/7
no switchport
ip address 172.16.0.13 255.255.255.252
exit

interface gig1/0/8
no switchport
ip address 172.16.0.9 255.255.255.252
exit

Enable Layer 3 Routing:
ip routing

# SVI and HSRP Configuration:
interface vlan 10
ip address 10.1.1.3 255.255.255.0
standby 10 ip 10.1.1.1
standby 10 priority 100
standby 10 preempt
no shutdown
exit

interface vlan 20
ip address 10.1.2.3 255.255.255.0
standby 20 ip 10.1.2.1
standby 20 priority 100
standby 20 preempt
no shutdown
exit

interface vlan 30
ip address 10.1.3.3 255.255.255.0
standby 30 ip 10.1.3.1
standby 30 priority 100
standby 30 preempt
no shutdown
exit

interface vlan 40
ip address 10.1.4.3 255.255.255.0
standby 40 ip 10.1.4.1
standby 40 priority 100
standby 40 preempt
no shutdown
exit

interface vlan 50
ip address 10.1.5.3 255.255.255.0
standby 50 ip 10.1.5.1
standby 50 priority 100
standby 50 preempt
no shutdown
exit

interface vlan 60
ip address 10.1.6.3 255.255.255.0
standby 60 ip 10.1.6.1
standby 60 priority 100
standby 60 preempt
no shutdown
exit

interface vlan 70
ip address 10.1.7.3 255.255.255.0
standby 70 ip 10.1.7.1
standby 70 priority 100
standby 70 preempt
no shutdown
exit

interface vlan 80
ip address 10.1.8.3 255.255.255.0
standby 80 ip 10.1.8.1
standby 80 priority 100
standby 80 preempt
no shutdown
exit

DHCP Relay:

interface vlan 10
ip helper-address 10.1.8.10
exit

interface vlan 20
ip helper-address 10.1.8.10
exit

interface vlan 30
ip helper-address 10.1.8.10
exit

interface vlan 40
ip helper-address 10.1.8.10
exit

interface vlan 50
ip helper-address 10.1.8.10
exit

interface vlan 60
ip helper-address 10.1.8.10
exit

interface vlan 70
ip helper-address 10.1.8.10
exit

interface vlan 80
ip helper-address 10.1.8.10
exit

# 7. Core Router Configuration:

The core routers were used to provide routed connectivity above the distribution layer and to form OSPF neighbors with the multilayer switches.

# 7.1 CORE-R1:
Purpose:

Core routing and OSPF connectivity with distribution layer.

Basic Configuration
enable
configure terminal
hostname CORE-R1
banner motd #No Authorised Access!!!#
no ip domain lookup

line console 0
password cisco
login
exit

enable password cisco123
service password-encryption

SSH Configuration:
configure terminal
ip domain name cisco.net
username admin password cisco
crypto key generate rsa
1024

line vty 0 15
login local
transport input ssh
exit

ip ssh version 2
do write memory

Interface Configuration:

Based on your /30 addressing plan, CORE-R1 connects to both multilayer switches.

interface gig0/0/0
ip address 172.16.0.2 255.255.255.252
no shutdown
exit

interface g0/0/1
ip address 172.16.0.10 255.255.255.252
no shutdown
exit

OSPF Configuration
router ospf 1
network 172.16.0.0 0.0.0.3 area 0
network 172.16.0.8 0.0.0.3 area 0

# 7.2 CORE-R2:
Purpose:

Redundant core routing and OSPF connectivity.

Basic Configuration
enable
configure terminal
hostname CORE-R2
banner motd #No Authorised Access!!!#
no ip domain lookup

line console 0
password cisco
login
exit

enable password cisco123
service password-encryption

SSH Configuration:
configure terminal
ip domain name cisco.net
username admin password cisco
crypto key generate rsa
1024

line vty 0 15
login local
transport input ssh
exit

ip ssh version 2
do write memory
Interface Configuration:
interface gig0/0/0
ip address 172.16.0.14 255.255.255.252
no shutdown
exit

interface gig0/0/1
ip address 172.16.0.6 255.255.255.252
no shutdown
exit
OSPF Configuration
router ospf 1
network 172.16.0.4 0.0.0.3 area 0
network 172.16.0.12 0.0.0.3 area 0

# 8. OSPF Configuration Summary:

OSPF was configured on both multilayer switches and core routers. 

VLAN interfaces were advertised and made passive, while the uplinks remained active for neighbor formation.

Example OSPF on Multilayer Switch:
router ospf 1
network 10.1.1.0 0.0.0.255 area 0
network 10.1.2.0 0.0.0.255 area 0
network 10.1.3.0 0.0.0.255 area 0
network 10.1.4.0 0.0.0.255 area 0
network 10.1.5.0 0.0.0.255 area 0
network 10.1.6.0 0.0.0.255 area 0
network 10.1.7.0 0.0.0.255 area 0
network 10.1.8.0 0.0.0.255 area 0

passive-interface vlan 10
passive-interface vlan 20
passive-interface vlan 30
passive-interface vlan 40
passive-interface vlan 50
passive-interface vlan 60
passive-interface vlan 70
passive-interface vlan 80


Explanation:
VLAN networks are advertised into OSPF
VLAN interfaces are passive because no OSPF neighbor is needed there
routed uplinks to routers stay active because OSPF neighbors are formed on those links.
# 9. DHCP and DNS Server Details

Device	IP Address	Role
DHCP Server	10.1.8.10	Assigns IP addresses
DNS Server	10.1.8.12	Resolves hostnames
System Server	10.1.8.13	Management / system use

Explanation:
the server room uses static addressing
client VLANs receive IP addresses through DHCP relay
DNS server is used for name resolution in the network.

# SSH configuration done at all Switches & Routers.

Config t
ip domain name cisco.net
crypto key generate rsa
username admin password cisco

line vty 0 15 
login local
transport input ssh
exit 
do wr

# 10. Verification Commands

These commands were used to verify VLANs, VTP, trunking, routing, and HSRP. Your notes include several of these commands.

show vtp status
show vlan brief
show interfaces trunk
show interfaces fa0/3 switchport
show ip route
show ip ospf neighbor
show standby brief

Explanation:
show vtp status verifies VTP domain, mode, and version
show vlan brief verifies VLANs and assigned ports
show interfaces trunk verifies trunk links
show interfaces fa0/3 switchport checks which VLAN is assigned to a specific port
show ip route verifies learned routes
show ip ospf neighbor verifies OSPF neighbor adjacency
show standby brief verifies HSRP active and standby roles

# 11. Conclusion

This project demonstrates the implementation of a campus network using industry-standard technologies.

Key features of this design include:

* Department-wise VLAN segmentation for better network organization
* Centralized VLAN management using VTP
* Redundant uplinks using trunk links between access and distribution layers
* Inter-VLAN routing using multilayer switches
* Gateway redundancy using HSRP for high availability
* Dynamic routing using OSPF
* Centralized IP address allocation using DHCP relay
* Integration of DHCP and DNS servers in the server network
* Enhanced security using port security and shutdown of unused ports

Overall, this topology represents a small enterprise-level campus network design with scalability, redundancy, and security.





