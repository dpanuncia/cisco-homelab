# Cisco CLI Quick Reference

## Navigation
| Command | What it does |
|---------|-------------|
| `enable` | Enter privileged mode (#) |
| `conf t` | Enter global config mode |
| `exit` | Go back one level |
| `end` | Go all the way back to privileged mode |
| `write memory` | Save config to flash |
| `show running-config` | Show current config |
| `show startup-config` | Show saved config |

## Interfaces
| Command | What it does |
|---------|-------------|
| `interface GigabitEthernet0/0` | Enter interface config |
| `ip address 192.168.1.1 255.255.255.0` | Set IP manually |
| `ip address dhcp` | Get IP via DHCP |
| `no shutdown` | Turn interface on |
| `shutdown` | Turn interface off |
| `show ip interface brief` | Quick status of all interfaces |

## VLANs (Switch)
| Command | What it does |
|---------|-------------|
| `vlan 10` | Create VLAN 10 |
| `name MAIN` | Name the VLAN |
| `switchport mode access` | Set port to access mode (one VLAN) |
| `switchport access vlan 10` | Assign port to VLAN 10 |
| `switchport mode trunk` | Set port to trunk mode (multiple VLANs) |
| `switchport trunk allowed vlan 10,20` | Allow specific VLANs on trunk |
| `show vlan brief` | Show all VLANs |
| `show interfaces trunk` | Show trunk ports |

## Routing
| Command | What it does |
|---------|-------------|
| `encapsulation dot1Q 10` | Tag subinterface with VLAN 10 |
| `ip route 0.0.0.0 0.0.0.0 [gateway]` | Default route |
| `show ip route` | Show routing table |

## DHCP
| Command | What it does |
|---------|-------------|
| `ip dhcp excluded-address 10.0.0.1 10.0.0.10` | Reserve addresses |
| `ip dhcp pool NAME` | Create DHCP pool |
| `network 10.0.0.0 255.255.255.0` | Define subnet |
| `default-router 10.0.0.1` | Set gateway |
| `dns-server 8.8.8.8` | Set DNS |
| `show ip dhcp binding` | Show assigned IPs |

## NAT
| Command | What it does |
|---------|-------------|
| `ip nat inside` | Tag interface as inside |
| `ip nat outside` | Tag interface as outside |
| `ip nat inside source list ACL interface GE0/0 overload` | Enable PAT |
| `show ip nat translations` | Show active NAT entries |

## SSH Setup
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

## Useful Show Commands
| Command | What it does |
|---------|-------------|
| `show version` | IOS version and hardware info |
| `show cdp neighbors` | Show directly connected devices |
| `show interfaces` | Detailed interface stats |
| `show ip interface brief` | Quick interface status |
| `show vlan brief` | VLAN summary |
| `show interfaces trunk` | Trunk port status |
| `show ip route` | Routing table |
| `show ip dhcp binding` | DHCP leases |
| `show ip nat translations` | NAT table |
| `show running-config` | Full current config |
