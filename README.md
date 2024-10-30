This repository is a guide for learn Juniper Junos® OS resources and references to practice configuration. 

# Initiation Command

## Command References Juniper Junos® OS

| Command | Description |
| ---- | ----- |
| show interfaces descriptions | show description in interface |  
| show interfaces brief | show active port interface |
| show interfaces terse | show interface up/down |
| show interfaces | show all interface |
| show route forwarding-table summary | show all route forwarding |
| show route summary | show all route |
| show bgp summary | show bgp all connected | 
| show bgp neighbor | show bgp neighbor connected | 
| show route receive-protocol bgp `IP_Destination` |
| show route advertising-protocol bgp `IP_Destination` |
| show ospf neighbor | show ospf neighbor connected | 
| show ospf interface | show ospf interface configuration |
| show mpls interface | show mpls interface configuration | 
| show mpls lsp brief | show mpls lsp connected |
| show ldp session | show all ldp connected |
| show ldp interface | show ldp interface configuration |
| show ldp neighbor | show ldp neighbor connected |
| show rsvp session | show rsvp connected |
| show rsvp interface | show rsvp interface configuration |
| show rsvp neighbor | show rsvp neighbor connected |

# Basic Configuration

## Hostname Device
```
set system host-name "[DEVICE_NAME]"
```

## Timezone
```
set system time-zone Asia/Jakarta
```

## Root Authentication Device
```
set system root-authentication password => ENTER
[password]
```

## Username and Password
```
set system login user [USERNAME] class admin
set system login user [USERNAME] authentication password => ENTER
[password]
```
```
set system login user [USERNAME] class super-user
set system login user [USERNAME] authentication password => ENTER
[password]
```
```
set system login user [USERNAME] class operator
set system login user [USERNAME] authentication password => ENTER
[password]
```

## System Service
```
set system services ftp
set system services ssh root-login deny
set system services ssh rate-limit 5
set system services telnet
```

# Interface Configuration

## Interface
Configuration Loopback
```
set interfaces lo0 unit 0 description [DESCRIPTION]
set interfaces lo0 unit 0 family inet address [IP_ADDRESS_NETMASK]
```
Configuration Port Interface
```
set interfaces ge-0/0/0 mtu [1900 - 9200]
set interfaces ge-0/0/0 unit 0 description [DESCRIPTION]
set interfaces ge-0/0/0 unit 0 family inet address [IP_ADDRESS_NETMASK]
set interfaces ge-0/0/0 unit 0 family mpls
```
Verification
```
show interface terse | grep up
show interface terse | grep down
show interface brief
```

# Routing OSPF Configuration

## OSPF (Open Shortest Path First)

Configuration Routing OSPF Single Area or Backbone Area
```
set protocols ospf area [ID_AREA] interface [PORT] interface-type p2p
set protocols ospf area [ID_AREA] interface [PORT] metric [1-65000]  <- MTU
set protocols ospf area [ID_AREA] interface [PORT] hello-interval 5
set protocols ospf area [ID_AREA] interface [PORT] dead-interval 15
set protocols ospf area [ID_AREA] interface [PORT] authentication md5 1 key [PASSWORD]
set protocols ospf area [ID_AREA] interface [LOOPBACK] interface-type p2p
set protocols ospf area [ID_AREA] interface [LOOPBACK] metric [1-65000]  <- MTU
```
Configuration Routing OSPF Multiarea
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
```
set protocols mpls interface [PORT_INTERFACE]
set protocols mpls interface [LOOPBACK]
```
Verification
```
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
run show ldp interface
run show ldp neighbor
run show ldp session
```
## LLDP (Link Layer Discovery Protocol)
```
set protocols lldp interface [PORT_INTERFACE]
set protocols lldp interface [PORT_INTERFACE]
```
Verification
```

```

## RSVP (Resource Reservation Protocol)
```
set protocols rsvp interface [PORT_INTERFACE]
set protocols rsvp interface [PORT_INTERFACE]
set protocols rsvp interface [LOOPBACK]
```
Verification
```

```

# BGP Configuration

## iBGP (Interior)
iBGP to BGP Route Reflector
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

## eBGP (Exterior)

# Comming Soon !!!

# MPLS L2VPN

## L2 Circuit
L2 Configuration
```
set protocols l2circuit neighbor [DESTINATION_LOOPBACK] interface ge-0/0/1.10 virtual-circuit-id 10
set protocols l2circuit neighbor [DESTINATION_LOOPBACK] interface ge-0/0/1.10 description L2VPN
set protocols l2circuit neighbor [DESTINATION_LOOPBACK] interface ge-0/0/1.10 no-control-word
set protocols l2circuit neighbor [DESTINATION_LOOPBACK] interface ge-0/0/1.10 ignore-encapsulation-mismatch
```
Port Service (Trunk Port)
```
set interfaces ge-0/0/1 flexible-vlan-tagging
set interfaces ge-0/0/1 mtu 1900
set interfaces ge-0/0/1 encapsulation flexible-ethernet-services
set interfaces ge-0/0/1 unit 10 description L2VPN
set interfaces ge-0/0/1 unit 10 encapsulation vlan-ccc
set interfaces ge-0/0/1 unit 10 vlan-id 10
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

## VPLS (Virtual Private LAN Service)

# MPLS L3VPN

## VRF (Virtual Routing Forwarding)

## VRF Option AS-Overide

# Additional Resources

## Books / Manuals

## Youtube


