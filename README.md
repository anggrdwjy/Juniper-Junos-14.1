# This repository is a guide for learn Juniper Junos® OS resources and references to practice configuration Router. 

## Summary
* [Command References](#Command-References)
* [Basic Configuration](#Basic-Configuration)
  * [Hostname Device](#Hostname-Device)
  * [Time Clock Device](#Time-Clock-Device)
  * [Root Authentication Device](#Root-Authentication-Device)
  * [Username and Password](Username-and-Password)
  * [System Service](#System-Service)
  * [NTP \(Network Time Protocol)](#NTP-(Network-Time-Protocol))
  * [SNMP \(Simple Network Management Protocol)](#SNMP-(Simple-Network-Management-Protocol))
* [Interface Configuration](#Interface-Configuration)
  * [Loopback Configuration](#Loopback-Configuration)
  * [Port Interface Configuration](#Port-Interface-Configuration)
* [LACP \(Link Aggregation Control Protocol)](#LACP-(Link-Aggregation-ControlProtocol))
  * [LACP Configuration](#LACP_Configuration)
  * [VLAN Tagging LACP](#VLAN_Tagging_LACP)
  * [Port Interface Configuration](#Port-Interface-Configuration)
* [Static Routing Configuration](#Static-Routing-Configuration)
  * [Static Routing](#Static-Routing)
* [Routing OSPF Configuration](#Routing-OSPF-Configuration)
  * [Routing OSPF Configuration](#Routing-OSPF-Configuration)
* [MPLS LDP, LLDP, RSVP Configuration](#MPLS-LDP-LLDP-RSVP-Configuration)
  * [MPLS \(Multi-Protocol Labeling Switching)](#MPLS-(Multi-Protocol-Labeling-Switching))
  * [LDP \(Label Distribution Protocol)](#LDP-(Label-Distribution-Protocol))
  * [LLDP \(Link Layer Discovery Protocol)](#LLDP-(Link-Layer-Discovery-Protocol))
  * [RSVP \(Resource Reservation Protocol)](#RSVP-(Resource-Reservation-Protocol))
* [Routing BGP Configuration](#Routing-BGP-Configuration)
  * [iBGP \(Interior)](#iBGP-(Interior))
* [MPLS L2VPN](#MPLS-L2VPN)
  * [L2 Circuit](#L2-Circuit)
  * [VPLS \(Virtual Private LAN Service)](#VPLS-(Virtual-Private-LAN-Service))
* [MPLS L3VPN](#MPLS-L3VPN)
  * [VRF \(Virtual Routing Forwarding)](#VRF-(Virtual-Routing-Forwarding))
  * [VRF Option Inter AS \(AS-Overide)](#VRF-Option-Inter-AS-(AS-Overide))
  * [VRF Option Inter OSPF \(Sham-Link)](#VRF-Option-Inter-OSPF-(Sham-Link))
* [Additional Resources](#Additional-Resources)
  * [Books / Manuals](#Books-/-Manuals)
  * [Youtube Playlist](#Youtube-Playlist)

## Command References

| Command | Description |
| ---- | ----- |
| show configuration system | show configuration in system routers |
| show configuration interface | show configuration in all interface |
| show interfaces descriptions | show description in interface |  
| show interfaces brief | show active port interface |
| show interfaces terse | show interface up/down |
| show interfaces | show all interface |
| show route forwarding-table summary | show all route forwarding |
| show route summary | show all route |
| show bgp summary | show bgp all connected | 
| show bgp neighbor | show bgp neighbor connected | 
| show bgp summary instance [VRF_NAME] | show bgp all connected from VRF |
| show ospf neighbor | show ospf neighbor connected | 
| show ospf interface | show ospf interface configuration |
| show ospf interface instance [VRF_NAME] | |
| show ospf neighbor instance [VRF_NAME] | |
| show mpls interface | show mpls interface configuration | 
| show mpls lsp brief | show mpls lsp connected |
| show lldp neighbors | show lldp neighbor connected |
| show lldp detail | show detail lldp |
| show ldp session | show all ldp connected |
| show ldp interface | show ldp interface configuration |
| show ldp neighbor | show ldp neighbor connected |
| show rsvp session | show rsvp connected |
| show rsvp interface | show rsvp interface configuration |
| show rsvp neighbor | show rsvp neighbor connected |
| request system snapshot | Backup Currently Running |
| request system configuration rescue save | Active File System |
| show route forwarding-table family inet | show route forwarding |
| show route forwarding-table family mpls | show route forwarding mpls |
| show route forwarding-table family vpls | show route forwarding vpls |
| show route forwarding-table vpn [VRF_NAME] | show route forwarding vpn |
| show route table [VRF_NAME].inet.0 | show routing table vpn |
| show l2circuit connections | show l2circuit neighbor connected |
| show vpls connections | show vpls neighbor connected |
| show vpls mac-table brief | show mac address table in vpls |
| ping [DESTINATION_IP] source [SOURCE_IP] | ping point to point |
| ping routing-instance [VRF_NAME] [DESTINATION_IP] source [SOURCE_IP] count 5 | ping vpn point to point |
| traceroute [DESTINATION_IP] source [SOURCE_IP] | trace route point to point |
| commit | save configuration |
| commit check | check and save configuration |


# Basic Configuration

## Hostname Device
```
set system host-name "[DEVICE_NAME]"
```

## Time Clock Device
```
set system time-zone Asia/Jakarta
```

## Root Authentication Device
```
set system root-authentication password => ENTER
[password]
```

## Username and Password
Admin User
```
set system login user [USERNAME] class admin
set system login user [USERNAME] authentication password => ENTER
[password]
```
Super User
```
set system login user [USERNAME] class super-user
set system login user [USERNAME] authentication password => ENTER
[password]
```
Operator User
```
set system login user [USERNAME] class operator
set system login user [USERNAME] authentication password => ENTER
[password]
```

## System Service
Activate FTP, TELNET and SSH
```
set system services ftp
set system services ssh root-login deny
set system services ssh rate-limit 5
set system services telnet
```

## NTP (Network Time Protocol)
NTP Configuration
```
set system ntp boot-server [NTP_SERVER_MAIN]
set system ntp server [NTP_SERVER_MAIN] prefer
set system ntp server [NTP_SERVER_BACKUP]
set system ntp source-address [IP_LOOPBACK]
```

## SNMP (Simple Network Management Protocol)
SNMP Configuration
```
set snmp description snmpfilter
set snmp location [HOSTNAME]
set snmp contact [MAIL@MAIL.COM]
set snmp community [SNMP-COMM] authorization read-only
set snmp community [SNMP-COMM] clients [TARGET_IP_A]
set snmp community [SNMP-COMM] clients [TARGET_IP_B]
set snmp community [SNMP-COMM] clients [TARGET_IP_C]
```
# Interface Configuration

## Interface
Loopback Configuration
```
set interfaces lo0 unit 0 description [DESCRIPTION]
set interfaces lo0 unit 0 family inet address [IP_ADDRESS_NETMASK]
```
Port Interface Configuration
```
set interfaces [PORT_INTERFACE] mtu [1900 - 9200]
set interfaces [PORT_INTERFACE] unit 0 description [DESCRIPTION]
set interfaces [PORT_INTERFACE] unit 0 family inet address [IP_ADDRESS_NETMASK]
set interfaces [PORT_INTERFACE] unit 0 family mpls
```
Verification
```
show interface terse | grep up
show interface terse | grep down
show interface brief
```
## LACP (Link Aggregation Control Protocol)
LACP Configuration
```
set interfaces ae3 description [DESCRIPTION]
set interfaces ae3 flexible-vlan-tagging
set interfaces ae3 mtu [1500 - 9000]
set interfaces ae3 aggregated-ether-options minimum-links 1
set interfaces ae3 aggregated-ether-options link-speed [BANDWIDTH_REFERENCE]
set interfaces ae3 aggregated-ether-options lacp active
```
VLAN Tagging LACP
```
set interfaces ae3 unit 123 description [DESCRIPTION]
set interfaces ae3 unit 123 vlan-id 123
set interfaces ae3 unit 123 family inet filter input [POLICY_FILTER]
set interfaces ae3 unit 123 family inet address [X.X.X.X/YY]
```
Port Interface Configuration
```
set interfaces [PORT_INTERFACE] description [DESCRIPTION]
set interfaces [PORT_INTERFACE] gigether-options no-flow-control
set interfaces [PORT_INTERFACE] gigether-options no-auto-negotiation
set interfaces [PORT_INTERFACE] gigether-options 802.3ad ae3
```
# Static Routing Configuration

## Static Routing
Static Routing Configuration
```
set routing-options static route [NETWORK_DESTINATION] 
set routing-options static route [NETWORK_DESTINATION] next-hop [NEXT_HOP]
```
Optional 
```
set routing-options static route [NETWORK_DESTINATION] discard
set routing-options static route [NETWORK_DESTINATION] preference 210
```

# Routing OSPF Configuration

## OSPF (Open Shortest Path First)
Optional OSPF Protocol
```
set protocols ospf traffic-engineering
set protocols ospf reference-bandwidth [CAPACITY_BANDWIDTH]
```
Optional Policy OSPF Protocol
```
set policy-options policy-statement [OSPF-TERM] term 1 from route-filter [X.X.X.X/YY] exact
set policy-options policy-statement [OSPF-TERM] term 1 from route-filter [X.X.X.X/YY] exact
set policy-options policy-statement [OSPF-TERM] term 1 from route-filter [X.X.X.X/YY] exact
set policy-options policy-statement [OSPF-TERM] term 1 then accept
set policy-options policy-statement [OSPF-TERM] term 2 then reject
set protocols ospf export [OSPF-TERM]
```
Routing OSPF Single Area / Backbone Area Configuration
```
set protocols ospf area [ID_AREA] interface [PORT] interface-type p2p
set protocols ospf area [ID_AREA] interface [PORT] metric [1-65000]  <- MTU
set protocols ospf area [ID_AREA] interface [PORT] hello-interval 5
set protocols ospf area [ID_AREA] interface [PORT] dead-interval 15
set protocols ospf area [ID_AREA] interface [PORT] authentication md5 1 key [PASSWORD]
set protocols ospf area [ID_AREA] interface [LOOPBACK] interface-type p2p
set protocols ospf area [ID_AREA] interface [LOOPBACK] metric [1-65000]  <- MTU
```
Routing OSPF Multiarea Configuration
```
set protocols ospf area [ID_AREA_A] interface [PORT] interface-type p2p
set protocols ospf area [ID_AREA_A] interface [PORT] metric [1-65000]  <- MTU
set protocols ospf area [ID_AREA_A] interface [PORT] hello-interval 5
set protocols ospf area [ID_AREA_A] interface [PORT] dead-interval 15
set protocols ospf area [ID_AREA_A] interface [PORT] authentication md5 1 key [PASSWORD]
set protocols ospf area [ID_AREA_A] interface [LOOPBACK] interface-type p2p
set protocols ospf area [ID_AREA_A] interface [LOOPBACK] metric [1-65000]  <- MTU
set protocols ospf area [ID_AREA_B] interface [PORT] interface-type p2p
set protocols ospf area [ID_AREA_B] interface [PORT] metric [1-65000]  <- MTU
set protocols ospf area [ID_AREA_B] interface [PORT] hello-interval 5
set protocols ospf area [ID_AREA_B] interface [PORT] dead-interval 15
set protocols ospf area [ID_AREA_B] interface [PORT] authentication md5 1 key [PASSWORD]
```
Verification
```
show ospf interface
show ospf neighbor
```

# MPLS LDP, LLDP, RSVP Configuration

## MPLS (Multi-Protocol Labeling Switching)
MPLS Interface Configuration
```
set protocols mpls no-propagate-ttl
set protocols mpls interface [PORT_INTERFACE]
set protocols mpls interface [LOOPBACK]
```
Verification
```
show route forwarding-table family mpls
show mpls interface brief
show mpls lsp brief

```
## LDP (Label Distribution Protocol)
LDP Interface and Neighbor Authentication
```
set protocols ldp interface [PORT_INTERFACE]
set protocols ldp interface [LOOPBACK]
set protocols ldp session [IP_NEIGHBOR_1] authentication-key [PASSWORD]
set protocols ldp session [IP_NEIGHBOR_2] authentication-key [PASSWORD]
set protocols ldp session [IP_NEIGHBOR_3] authentication-key [PASSWORD]
```
Verification
```
show ldp interface
show ldp neighbor
show ldp session
```
## LLDP (Link Layer Discovery Protocol)
LLDP Configuration
```
set protocols lldp interface [PORT_INTERFACE]
set protocols lldp interface [PORT_INTERFACE]
```
Verification
```
show lldp neighbor
show lldp detail
```

## RSVP (Resource Reservation Protocol)
RSVP Configuration
```
set protocols rsvp interface [PORT_INTERFACE]
set protocols rsvp interface [PORT_INTERFACE]
set protocols rsvp interface [LOOPBACK]
```
Verification
```
show rsvp interface
```

# Routing BGP Configuration

## iBGP (Interior) Example
BGP Configuration to Route Reflector Client
```
set routing-options graceful-restart
set routing-options router-id [IP_LOOPBACK]
set routing-options autonomous-system [AS-NUMBER]
set protocols bgp log-updown
set protocols bgp group [BGP_GROUP_RR_CLIENT] type internal
set protocols bgp group [BGP_GROUP_RR_CLIENT] local-address [IP_LOOPBACK]
set protocols bgp group [BGP_GROUP_RR_CLIENT] family inet-vpn unicast  <- VPNV4
set protocols bgp group [BGP_GROUP_RR_CLIENT] authentication-key [PASSWORD]
set protocols bgp group [BGP_GROUP_RR_CLIENT] cluster [CLUSTER_ID]
set protocols bgp group [BGP_GROUP_RR_CLIENT] peer-as [AS_NUMBER_PEERING]
set protocols bgp group [BGP_GROUP_RR_CLIENT] neighbor [IP_RR_CLIENT_A] description [DESCRIPTION]
set protocols bgp group [BGP_GROUP_RR_CLIENT] neighbor [IP_RR_CLIENT_B] description [DESCRIPTION]
set protocols bgp group [BGP_GROUP_RR_CLIENT] neighbor [IP_RR_CLIENT_C] description [DESCRIPTION]
set protocols bgp group [BGP_GROUP_RR_CLIENT] neighbor [IP_RR_CLIENT_D] description [DESCRIPTION]
```
BGP Configuration to Route Reflector
```
set routing-options graceful-restart
set routing-options router-id [IP_LOOPBACK]
set routing-options autonomous-system [AS-NUMBER]
set protocols bgp log-updown
set protocols bgp group [BGP_GROUP] type internal
set protocols bgp group [BGP_GROUP] local-address [IP_LOOPBACK]
set protocols bgp group [BGP_GROUP] family inet-vpn unicast  <- VPNV4
set protocols bgp group [BGP_GROUP] authentication-key [PASSWORD]
set protocols bgp group [BGP_GROUP] cluster [CLUSTER_ID]
set protocols bgp group [BGP_GROUP] peer-as [AS_NUMBER_PEERING]
set protocols bgp group [BGP_GROUP] neighbor [IP_ROUTE_REFLECTOR] description [DESCRIPTION]
```
Verification
```
show bgp summary 
show bgp neighbor
```

# MPLS L2VPN

## L2 Circuit Example
L2 Circuit Configuration Example
```
set protocols l2circuit neighbor [DESTINATION_LOOPBACK] interface ge-0/0/1.10 virtual-circuit-id 10
set protocols l2circuit neighbor [DESTINATION_LOOPBACK] interface ge-0/0/1.10 description L2VPN
set protocols l2circuit neighbor [DESTINATION_LOOPBACK] interface ge-0/0/1.10 no-control-word
set protocols l2circuit neighbor [DESTINATION_LOOPBACK] interface ge-0/0/1.10 ignore-encapsulation-mismatch
```
Port Service (Access Port)
```
set interfaces ge-0/0/1 flexible-vlan-tagging
set interfaces ge-0/0/1 mtu 1900
set interfaces ge-0/0/1 encapsulation flexible-ethernet-services
set interfaces ge-0/0/1 unit 10 description L2VPN
set interfaces ge-0/0/1 unit 10 encapsulation vlan-ccc
set interfaces ge-0/0/1 unit 10 vlan-id 10
```
Verification
```
show l2circuit connections
```

## VPLS (Virtual Private LAN Service) Example
VPLS Configuration
```
set routing-instances NODE-XYZ instance-type vpls 
set routing-instances NODE-XYZ interface ge-0/0/4.0 
set routing-instances NODE-XYZ protocols vpls encapsulation-type ethernet 
set routing-instances NODE-XYZ protocols vpls no-control-word 
set routing-instances NODE-XYZ protocols vpls no-tunnel-services 
set routing-instances NODE-XYZ protocols vpls vpls-id 234 
set routing-instances NODE-XYZ protocols vpls mtu 1500 
set routing-instances NODE-XYZ protocols vpls ignore-mtu-mismatch 
set routing-instances NODE-XYZ protocols vpls ignore-encapsulation-mismatch 
set routing-instances NODE-XYZ protocols vpls neighbor 192.168.0.2 switchover-delay 5000 
set routing-instances NODE-XYZ protocols vpls neighbor 192.168.0.2 revert-time 60 
set routing-instances NODE-XYZ protocols vpls neighbor 192.168.0.2 backup-neighbor 192.168.0.1 standby 
```
Port Service (Access)
```
set interfaces ge-0/0/4 unit 0 description "HO-XYZ" 
set interfaces ge-0/0/4 mtu 1500 
set interfaces ge-0/0/4 encapsulation ethernet-vpls 
```
Verification
```
show vpls connections
show vpls mac-table brief
show route forwarding-table family vpls
```

# MPLS L3VPN

## VRF (Virtual Routing Forwarding) Example
VPN Policy
```
set policy-options policy-statement VPN-SERVER-XYZ-EXPORT term 1 then community add target:65000:100 
set policy-options policy-statement VPN-SERVER-XYZ-EXPORT term 1 then accept 
set policy-options policy-statement VPN-SERVER-XYZ-IMPORT term 1 from community target:65000:100 
set policy-options policy-statement VPN-SERVER-XYZ-IMPORT term 1 then accept 
set policy-options policy-statement VPN-SERVER-XYZ-IMPORT term other then reject 
set policy-options community target:65000:100 members target:65000:100
```
VPN Instance
```
set routing-instances VPN-SERVER-XYZ instance-type vrf 
set routing-instances VPN-SERVER-XYZ interface ge-0/0/1.0 
set routing-instances VPN-SERVER-XYZ route-distinguisher 65000:100 
set routing-instances VPN-SERVER-XYZ vrf-import VPN-SERVER-XYZ-IMPORT 
set routing-instances VPN-SERVER-XYZ vrf-export VPN-SERVER-XYZ-EXPORT 
set routing-instances VPN-SERVER-XYZ vrf-table-label 
```
Port Service (Access)
```
set interfaces ge-0/0/1 unit 0 description "to SERVER-XYZ" 
set interfaces ge-0/0/1 encapsulation ethernet 
set interfaces ge-0/0/1 mtu 1500 
set interfaces ge-0/0/1 unit 0 family inet address 192.110.0.1/30 
```

## VRF Option Inter AS (AS-Overide) Example
VPN Policy
```
set policy-options policy-statement VPN-SERVER-12-EXPORT term 1 then community add target:65000:200 
set policy-options policy-statement VPN-SERVER-12-EXPORT term 1 then accept 
set policy-options policy-statement VPN-SERVER-12-IMPORT term 1 from community target:65000:200 
set policy-options policy-statement VPN-SERVER-12-IMPORT term 1 then accept 
set policy-options policy-statement VPN-SERVER-12-IMPORT term other then reject 
set policy-options community target:65000:200 members target:65000:200 
```
VPN Instance
```
set routing-instances VPN-SERVER-12 instance-type vrf 
set routing-instances VPN-SERVER-12 interface ge-0/0/3.0 
set routing-instances VPN-SERVER-12 route-distinguisher 65000:200 
set routing-instances VPN-SERVER-12 vrf-import VPN-SERVER-12-IMPORT 
set routing-instances VPN-SERVER-12 vrf-export VPN-SERVER-12-EXPORT 
set routing-instances VPN-SERVER-12 vrf-table-label
```
Redistribute BGP
```
set routing-instances VPN-SERVER-12 protocol bgp group VPN-SERVER-12 type external 
set routing-instances VPN-SERVER-12 protocol bgp group VPN-SERVER-12 family inet unicast 
set routing-instances VPN-SERVER-12 protocol bgp group VPN-SERVER-12 neighbor 172.100.0.2 
set routing-instances VPN-SERVER-12 protocol bgp group VPN-SERVER-12 neighbor 172.100.0.2 as-override
set routing-instances VPN-SERVER-12 protocol bgp group VPN-SERVER-12 neighbor 172.100.0.2 peer-as 65488 
set routing-instances VPN-SERVER-12 protocol bgp group VPN-SERVER-12 neighbor 172.100.0.2 local-as 65000 
set routing-instances VPN-SERVER-12 protocol bgp group VPN-SERVER-12 neighbor 172.100.0.2 export EXPORT-AS65488 
set policy-options policy-statement EXPORT-AS65488 term 1 from route-filter 202.20.0.0/30 exact 
set policy-options policy-statement EXPORT-AS65488 term 1 then accept 
set policy-options policy-statement EXPORT-AS65488 term other then reject 
```
Port Service (Access)
```
set interfaces ge-0/0/3 unit 0 description "to SERVER-12" 
set interfaces ge-0/0/3 encapsulation ethernet 
set interfaces ge-0/0/3 mtu 1500 
set interfaces ge-0/0/3 unit 0 family inet address 172.100.0.1/30 
```
Verification
```
show route table [VRF_NAME].inet.0
show route forwarding-table vpn [VRF_NAME]
ping routing-instance [VRF_NAME] [DESTINATION_IP] source [SOURCE_IP] count 5
show bgp summary instance [VRF_NAME]
ping [DESTINATION_IP] source [SOURCE_IP]
traceroute [DESTINATION_IP] source [SOURCE_IP]
```

## VRF Option Inter OSPF (Sham-Link) Example
VPN Policy
```
set policy-options policy-statement VPN-OSPF-AB-EXPORT term 1 then community add target:65000:300 
set policy-options policy-statement VPN-OSPF-AB-EXPORT term 1 then accept 
set policy-options policy-statement VPN-OSPF-AB-IMPORT term 1 from community target:65000:300 
set policy-options policy-statement VPN-OSPF-AB-IMPORT term 1 then accept 
set policy-options policy-statement VPN-OSPF-AB-IMPORT term other then reject 
set policy-options community target:65000:300 members target:65000:300 
```
VPN Instance
```
set routing-instances VPN-OSPF-AB instance-type vrf 
set routing-instances VPN-OSPF-AB interface ge-0/0/3.0 
set routing-instances VPN-OSPF-AB interface lo0.1 
set routing-instances VPN-OSPF-AB route-distinguisher 65000:300 
set routing-instances VPN-OSPF-AB vrf-import VPN-OSPF-AB-IMPORT 
set routing-instances VPN-OSPF-AB vrf-export VPN-OSPF-AB-EXPORT 
set routing-instances VPN-OSPF-AB vrf-table-label 
set routing-instances VPN-OSPF-AB protocol ospf export EXPORT-OSPF-888 
set routing-instances VPN-OSPF-AB protocol ospf sham-link local 103.22.0.253 
set routing-instances VPN-OSPF-AB protocol ospf area 0.0.3.120 sham-link-remote 103.32.0.253 metric 10 
set routing-instances VPN-OSPF-AB protocol ospf area 0.0.3.120 interface ge-0/0/3.0 
set routing-instances VPN-OSPF-AB protocol ospf area 0.0.3.120 interface lo0.1 
```
Redistribute OSPF
```
set policy-options policy-statement EXPORT-OSPF-888 term 1 from protocol bgp 
set policy-options policy-statement EXPORT-OSPF-888 term 1 then accept 
set policy-options policy-statement EXPORT-OSPF-888 term other then reject 
```
Port Service (Access)
```
set interfaces lo0 unit 1 family inet address 103.22.0.253/32 
set interfaces ge-0/0/3 unit 0 description "to NODE-A" 
set interfaces ge-0/0/3 encapsulation ethernet 
set interfaces ge-0/0/3 mtu 1500 
set interfaces ge-0/0/3 unit 0 family inet address 10.0.0.1/30 
```
Verification
```
show ospf interface instance [VRF_NAME]
show ospf neighbor instance [VRF_NAME]
ping [DESTINATION_IP] source [SOURCE_IP]
traceroute [DESTINATION_IP] source [SOURCE_IP]
```

# Additional Resources

## Books / Manuals
- [Juniper MPLS and Service by Anggarda Saputra Wijaya](https://drive.google.com/file/d/1RCGwMK9uCFW9ju0MRgpeNHQmXs1LZVmZ/view?usp=drive_link)
- [Junos CLI Guide](https://drive.google.com/file/d/1nTZlcABOvOkjPxvqIL1U7ZGQMirrm5XA/view?usp=drive_link)
- [Workbook Juniper Logical System and Routing Protocols](https://drive.google.com/file/d/1OL0xcsyCosylRMwiZLvtqNWKAi1-9jbm/view?usp=drive_link)
- [Advanced Juniper Networks Routing](https://drive.google.com/file/d/1sCcvM2h_k7ehJqFLzxEikmGd4bb5kXGe/view?usp=drive_link)
- [JNCIA Study Guide](https://drive.google.com/file/d/1NV1KoVSsw8fiSgKXCKXpbGs9ZgbirCZa/view?usp=drive_link)
- [JNCIS Study Guide](https://drive.google.com/file/d/1C_fswdTmXDRz1OTheNgJx9UqizCH2BJf/view?usp=drive_link)
- [JNCIP Study Guide](https://drive.google.com/file/d/1To8lD-9dcexJhC2-OLYGCgWqOXPRYZeN/view?usp=drive_link)
- [JNCIE Study Guide](https://drive.google.com/file/d/1W2PyTqB0W2p8BwgFwejNcJ624g54hepi/view?usp=drive_link)
  
## Youtube Playlist
- [Juniper Router vMX Junos 14.1 Routing OSPF, MPLS, BGP, and Services](https://www.youtube.com/playlist?list=PLy064HwEq9IyTJ9UQzxQ3GHHSa_S7d9DG)
