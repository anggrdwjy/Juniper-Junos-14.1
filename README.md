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
run show interface terse | grep up
run show interface terse | grep down
run show interface brief
```


# Routing OSPF Configuration

## OSPF (Open Shortest Path First)

# MPLS LDP, LLDP, RSVP Configuration

## MPLS (Multi-Protocol Labeling Switching)

## LDP (Label Distribution Protocol)

## LLDP (Link Layer Discovery Protocol)

## RSVP (Resource Reservation Protocol)

# BGP Configuration

## iBGP (Interior)

## eBGP (Exterior)

# MPLS L2VPN

## L2 Circuit

## VPLS (Virtual Private LAN Service)

# MPLS L3VPN

## VRF (Virtual Routing Forwarding)

## VRF Option AS-Overide

# Additional Resources

## Books / Manuals

## Youtube


