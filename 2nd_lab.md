# 2nd lab - Multicast lab - IGMP and PMI

### Background

#### IGMP (Internet Group Management Protocol) 
Protokol, který umožní se koncové stanice registrovat –připojit k požadované multicastové skupině a odebírat daný tok 

* IGMP snooping – každý přepínač monitoruje IGMP zprávy přicházející od koncových systémů a na jejich základě povolí či zakáže posílání odpovídajících ethernet multicast rámců (jinak by přepínače spamovaly celou LAN síť, kdyby každý multicast posílaly dál) 

#### PIM (Protocol Independant Multicast) 
Interní multicastový směrovací protokol, který umožňuje nalezení optimálních cest v sítí, po nichž se následně budou přenášet IP multicastové pakety

* Dense PIM - metoda směrování nejkratšími cestami od zdroje k příjemcům –„Shortest Paths Tree"
* Sparse PIM - metoda směrování na základě sdíleného distribučního stromu –„Shared tree“

### Lab 7-1 Implementing IGMP and IGMP Snooping ([PDF](tutorials/2%20-%20IP%20multicast%2C%20IPv6/CCNP1_lab_7_1_en.pdf))
*In this example R1 receives multicast packets, R2 sends them and switch SW1 is between them*

*DEBUG: To see debug logs:*
```
R1# debug ip igmp
R1# debug ip packet
```
---
To join a group receiveing multicast 229.7.7.7 on fa0/0
```
R1(config)# interface fastethernet 0/0 
R1(config-if)# ip igmp join-group 229.7.7.7
```

To see information about igmp on fa0/0, issue this command:
```
R1# show ip igmp interface fa0/0
```

To Display the IGMP groups to which the device have subscribed:
```
R1# show ip igmp groups
R1# show ip igmp membership
```

To test whether the listener device actually receives and responds to multicats packets:
```
R2# ping 229.7.7.7
```

#### Setup IGMP snooping on switch

To see whether IGMP snooping is enabled (it usually is by default):
```
SW1# show ip igmp snooping
```

IGMP snooping only subscribes other switchports to multicast groups at Layer 2 if it sees IGMP messages sent to the multicast router. Until you configure a supported multicast routing protocol (PIM or DVMRP) on the multicast router, IGMP snooping does not create MAC address table entries for the multicast groups.

You can also check table with registered MAC addresses for forwarding multicast packets:
```
SW1# show mac address-table multicast
```
##### Configure a Multicast-Enabled Router on the VLAN

First, enable (if disabled) IP routing on router with the ip routing command. 
```
R2(config)# ip routing
```

Secondly, enable the multicast-routing:
```
R2(config)# ip multicast-routing
```


Enable PIM-DM on R2’s fa0/0:
```
R2(config-if)# ip pim dense-mode
```

And voilá

### Lab 7-2 Routing IP Multicast with PIM Dense Mode ([pdf](tutorials/2%20-%20IP%20multicast%2C%20IPv6/CCNP1_lab_7_2_en.pdf))

In previous lab, nothing extra had to be made on switch, because switch (if snooping is disabled) treats multicast packets as broadcast. That's why it broadcasted everything and multicast packets eventually got to its destination

In topology with multiple routers, routers have to known ip routing to find optimal paths when forwarding multicast packets - **that's why first EIGRP (or OSPF) must be setup**.

Then to enable multicast routing, run this on every router in the topology:
```
R(config)# ip multicast-routing
```

Finally, enable PIM-DM on every interface in the topology, which should participate on multicast routing (even e.g. the loopback interfaces, which should be destination for multicast packets):
```
R1(config)# interface lo1
R1(config-if)# ip pim dense-mode
R1(config-if)# interface fa0/0
R1(config-if)# ip pim dense-mode
...
```

##### Debug
Display the multicast routing table on each router with: 
```
R1# show ip mroute
```

[In the lab PDF](tutorials/2%20-%20IP%20multicast%2C%20IPv6/CCNP1_lab_7_2_en.pdf), one can find **MANY** print/show commands to verify how the topology actuall works - starting in Step4.


### Lab 7-3 Routing IP Multicast with PIM Sparse Mode ([PDF](tutorials/2%20-%20IP%20multicast%2C%20IPv6/CCNP1_lab_7_3_en.pdf))

As before, the unicast routing must be working, so **firstly setup EIGRP (or OSPF)** and enable multicast routing on each router:
```
R(config)# ip multicast-routing
```

PIM sparse mode operates with Rendezvous point (RP), which is statically configured and which sould be in the middle of the topology. It "knows" all the multicast sources (but the RP router itself does not have to be in multicast communication tree, if it is not part of the shortest path)

To set Address `192.168.1.1` as the RP for multicast group `232.32.32.32`, run this on every router in the topology:
```
R1# conf t
R1(config)# access-list 32 permit 232.32.32.32
R1(config)# ip pim rp-address 192.168.1.1 32
R2# conf t
...
```

Finally, enable PIM sparse-mode on every interface involved in the communication:
```
R1(config)# interface lo1
R1(config-if)# ip pim sparse-mode
R1(config-if)# interface fa0/0
R1(config-if)# ip pim sparse-mode
...
```

##### Debug
Again, to display the multicast routing table on each router: 
```
R1# show ip mroute
```

Issue the show ip pim neighbors command to display all adjacent PIM routers:
```
R1# show ip pim neighbor
```

[In the lab PDF](tutorials/2%20-%20IP%20multicast%2C%20IPv6/CCNP1_lab_7_3_en.pdf), one can find **MANY** print/show commands to verify how the topology actually works - starting in Step4.



TODO
