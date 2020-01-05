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
* 

## BGP

