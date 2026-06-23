# Project 1 — VLAN Segmentation

## Status
Complete

## Objective
Segment homelab network using VLANs to isolate IoT devices from trusted traffic.

## Hardware Used
- Cisco Catalyst 2960-S (SW1 and SW2)
- Cisco 2901 ISR (R1)
- UniFi UDM Pro

## What is a VLAN?
A VLAN is a virtual wall inside a switch. Without VLANs every device on the network can see each other — your laptop, smart TV, Alexa, all in the same room. VLANs separate them into isolated groups even though they share the same physical hardware.

Think of it like a school building. VLANs are walls between classrooms. Students in one class can't walk into another class without permission.

## Commands — SW1 and SW2

### Enter config mode
```
enable
conf t
```

### Set hostname
```
hostname SW1
```

### Create VLANs
```
vlan 10
name MAIN
exit
vlan 20
name IOT
exit
```

### Set management IP
```
interface vlan 1
ip address 192.168.1.90 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.1.1
```

### Set up SSH
```
ip domain-name homelab.local
crypto key generate rsa modulus 2048
ip ssh version 2
username admin privilege 15 secret cisco123
line vty 0 15
login local
transport input ssh
exit
```

### Save
```
end
write memory
```

## Verification
```
show vlan brief
show ip interface brief
show interfaces trunk
```

## Skills Demonstrated
- VLAN configuration
- Cisco IOS CLI
- SSH setup with RSA encryption
- Network security principles
- Layer 2 switching

## CCNA Topics Covered
- VLANs (exam topic)
- SSH configuration (exam topic)
- Privilege levels (exam topic)
- Management IP on SVI (exam topic)
