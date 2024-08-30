# How To Configuration Juniper vMX Junos 14.1

Full Demo Juniper vMX Junos 14.1
---------------


Basic Configuration :
---------------
Basic Configuration Juniper
```
#Configuration Hostname
set system host-name [DEVICE_NAME]

#Set Timezone
set system time-zone [LOCATION]

#Password Root
set system root-authentication password => [PASSWORD]

#Configuration Username and Password
set system login user [USERNAME] class super-user
set system login user [USERNAME] authentication password => [PASSWORD]
set system login user [USERNAME] class operator
set system login user [USERNAME] authentication password => [PASSWORD]

#Privilage Login
set system services telnet
```
Verification :
```
telnet [IP_ADDRESS]
```

Interface Configuration :
---------------
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
show interface
```

Routing OSPF :
---------------
Configuration Routing OSPF
```
set protocols ospf area 0.0.0.0 interface ge-0/0/0.0 interface-type p2p
set protocols ospf area 0.0.0.0 interface ge-0/0/0.0 metric 1
set protocols ospf area 0.0.0.0 interface ge-0/0/0.0 hello-interval 5
set protocols ospf area 0.0.0.0 interface ge-0/0/0.0 dead-interval 15
set protocols ospf area 0.0.0.0 interface ge-0/0/0.0 authentication md5 1 key "$9$0xm11ES-ds4oGvWLNdVY25Qz6tu"
set protocols ospf area 0.0.0.0 interface lo0.0 interface-type p2p
set protocols ospf area 0.0.0.0 interface lo0.0 metric 65000
set protocols ospf area 0.0.0.0 interface ge-0/0/1.0 interface-type p2p
set protocols ospf area 0.0.0.0 interface ge-0/0/1.0 metric 1
set protocols ospf area 0.0.0.0 interface ge-0/0/1.0 hello-interval 5
set protocols ospf area 0.0.0.0 interface ge-0/0/1.0 dead-interval 15
set protocols ospf area 0.0.0.0 interface ge-0/0/1.0 authentication md5 1 key "$9$0ybt1ES-ds4oGvWLNdVY25Qz6tu"
```

Routing OSPF :
---------------
Configuration MPLS LDP :
```
set protocols mpls interface [PORT_INTERFACE]
set protocols mpls interface [LOOPBACK]
set protocols ldp interface [PORT_INTERFACE]
set protocols ldp interface [LOOPBACK]
set protocols ldp session [IP_NEIGHBOR] authentication-key [PASSWORD]
```

Configuration LLDP and RSVP :
---------------
```
set protocols lldp interface [PORT_INTERFACE]
set protocols lldp interface [PORT_INTERFACE]
set protocols rsvp interface ge-0/0/0.0
set protocols rsvp interface ge-0/0/1.0
set protocols rsvp interface lo0.0
```

Configuration BGP :
---------------
```
#Routing BGP
set routing-options graceful-restart
set routing-options router-id 1.1.1.1
set routing-options autonomous-system 65006

#Peering BGP
set protocols bgp log-updown
set protocols bgp group RR type internal
set protocols bgp group RR local-address 1.1.1.1
set protocols bgp group RR family inet-vpn unicast
set protocols bgp group RR authentication-key "$9$eWFML7ZGi.mTwYgJGUHkp0ORyl"
set protocols bgp group RR cluster 1.1.1.1
set protocols bgp group RR peer-as 65006
set protocols bgp group RR neighbor 6.6.6.6 description "To RR"
```










