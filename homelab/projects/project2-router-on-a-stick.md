# Project 2 — Router-on-a-Stick (Inter-VLAN Routing)

## Status
Complete

## Objective
Enable controlled communication between VLANs using a single physical link between SW1 and R1.

## Hardware Used
- Cisco Catalyst 2960-S (SW1)
- Cisco 2901 ISR (R1)

## What is Router-on-a-Stick?
VLANs are isolated — devices in VLAN 10 can't talk to devices in VLAN 20 by default. To allow controlled communication between VLANs you need a router.

Router-on-a-Stick uses ONE physical cable between the switch and router. That cable carries multiple VLANs by tagging each packet with a VLAN ID (dot1Q). The router has a subinterface for each VLAN — each one acts as the default gateway for that VLAN.

Think of it as a highway with multiple lanes. One road, multiple lanes, each lane is a different VLAN.

## Commands — R1

### Configure WAN interface (GE0/0)
```
interface GigabitEthernet0/0
description WAN - Connected to SW1
ip address dhcp
no shutdown
exit
```

### Configure trunk interface (GE0/1)
```
interface GigabitEthernet0/1
description LAN - Connected to R2
no ip address
no shutdown
exit
```

### Create subinterfaces
```
interface GigabitEthernet0/1.10
description VLAN 10 - MAIN
encapsulation dot1Q 10
ip address 10.10.10.1 255.255.255.0
no shutdown
exit

interface GigabitEthernet0/1.20
description VLAN 20 - IOT
encapsulation dot1Q 20
ip address 10.10.20.1 255.255.255.0
no shutdown
exit
```

## Commands — SW1

### Find port connected to R1
```
show cdp neighbors
```

### Configure trunk port
```
interface GigabitEthernet1/0/1
switchport mode trunk
switchport trunk allowed vlan 10,20
no shutdown
exit
write memory
```

## Verification
```
show ip interface brief
show interfaces trunk
show cdp neighbors
```

## Skills Demonstrated
- Router-on-a-Stick architecture
- 802.1Q trunking (dot1Q encapsulation)
- Subinterface configuration
- Trunk port configuration
- Layer 3 inter-VLAN routing

## CCNA Topics Covered
- Inter-VLAN routing (exam topic)
- 802.1Q encapsulation (exam topic)
- Subinterfaces (exam topic)
- Trunk ports (exam topic)
- CDP (exam topic)
