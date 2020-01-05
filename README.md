# Cisco Networking Academy
Notes for the exam of PST course. Document contains useful commands for the routing configuration and it is structured for the fast search of commands.

## Common Masks
* 255.255.0.0 = 16
* 255.255.252.0 = 22
* 255.255.255.0 = 24
* 255.255.255.252 = 30
* 255.255.255.255 = 32

## Common commands

* configuration mode `conf t`
* router ip address
```bash
# f0/0 or loop as interface
int loop0
# ip address and mask
ip addr 192.168.1.1 255.255.255.252 
# enable the interface
no shutdown
```
* show routing tables
```bash
show ip route
```

## BGP

* set up bgp routing
```bash
# enable routing bgp for router with `AS 100`
router bgp 100
# add neighbor on IP with `AS 300`
neighbor 192.168.1.6 remote-as 300
# set Loop IP on current machine
network 201.0.0.0
``` 
* show bgp neighbors
```bash
show ip bgp neighbors
```
* show bgp routing tables
```bash
show ip bgp
```
* remove private AS numbers from AS_Path configuration from Customer -> middle router striping from all other routers
 ```bash
 # on middle router and IP is IP of all other routers except the customer
neighbor 192.168.1.5 remove-private-as
 ```
* clear BGP records
```bash
clear ip bgp *
``` 

