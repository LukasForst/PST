# 2nd lab - Multicast lab - IGMP and PMI

### Background

#### IGMP (Internet Group Management Protocol) 
Protokol, který umožní se koncové stanice registrovat –připojit k požadované multicastové skupině a odebírat daný tok 

* IGMP snooping – každý přepínač monitoruje IGMP zprávy přicházející od koncových systémů a na jejich základě povolí či zakáže posílání odpovídajících ethernet multicast rámců (jinak by přepínače spamovaly celou LAN síť, kdyby každý multicast posílaly dál) 

#### PIM (Protocol Independant Multicast) 
Interní multicastový směrovací protokol, který umožňuje nalezení optimálních cest v sítí, po nichž se následně budou přenášet IP multicastové pakety

* Dense PIM - metoda směrování nejkratšími cestami od zdroje k příjemcům –„Shortest Paths Tree"
* Sparse PIM - metoda směrování na základě sdíleného distribučního stromu –„Shared tree“

### Lab 7-1 Implementing IGMP and IGMP Snooping
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

### Lab 7-2 Routing IP Multicast with PIM Dense Mode

TODO
