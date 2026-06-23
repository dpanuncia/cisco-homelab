# Project 3 — NAT/PAT & DHCP Server

## Status
Complete

## Objective
Configure R1 as a DHCP server for both VLANs and implement NAT/PAT so internal devices can reach the internet.

## Hardware Used
- Cisco 2901 ISR (R1)
- UniFi UDM Pro (upstream gateway)

## What is DHCP?
DHCP automatically assigns IP addresses to devices. Without it you'd manually assign an IP to every device. R1 acts as the DHCP server — when a device connects to VLAN 10 or VLAN 20, R1 hands it an IP, gateway, and DNS automatically.

Think of it as a hotel receptionist assigning room numbers to guests as they check in.

## What is NAT/PAT?
Your internal VLANs use private addresses (10.10.10.x, 10.10.20.x) that the internet doesn't recognize. NAT translates those private addresses to R1's public-facing IP when traffic leaves your network. When responses come back, NAT sends them to the right internal device.

PAT (Port Address Translation) lets multiple devices share one public IP — which is exactly what happens in your homelab.

Think of R1 as a post office. It receives mail for everyone inside and translates addresses in both directions.

## Commands — R1

### DHCP pools
```
ip dhcp excluded-address 10.10.10.1 10.10.10.10
ip dhcp pool VLAN10
network 10.10.10.0 255.255.255.0
default-router 10.10.10.1
dns-server 8.8.8.8
exit

ip dhcp excluded-address 10.10.20.1 10.10.20.10
ip dhcp pool VLAN20
network 10.10.20.0 255.255.255.0
default-router 10.10.20.1
dns-server 8.8.8.8
exit
```

### NAT access list
```
ip access-list standard NAT_ACL
permit 10.10.10.0 0.0.0.255
permit 10.10.20.0 0.0.0.255
exit
```

### Apply NAT/PAT
```
ip nat inside source list NAT_ACL interface GigabitEthernet0/0 overload
```

### Tag interfaces
```
interface GigabitEthernet0/0
ip nat outside
exit
interface GigabitEthernet0/1.10
ip nat inside
exit
interface GigabitEthernet0/1.20
ip nat inside
exit
```

### Save
```
end
write memory
```

## Verification
```
show ip interface brief
show ip dhcp pool
show ip dhcp binding
show ip nat translations
```

## Key Concepts
- **Wildcard mask** — opposite of subnet mask, used in ACLs. 0.0.0.255 means match any host in that /24 network
- **Overload** — allows multiple devices to share one public IP (PAT)
- **Inside/Outside** — NAT needs to know which side is internet and which is private

## Skills Demonstrated
- DHCP server configuration
- NAT/PAT implementation
- Standard access control lists
- WAN/LAN interface concepts
- Wildcard masks

## CCNA Topics Covered
- DHCP (exam topic)
- NAT/PAT (exam topic)
- Standard ACLs (exam topic)
- Wildcard masks (exam topic)
- WAN concepts (exam topic)
