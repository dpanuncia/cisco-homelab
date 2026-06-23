# Homelab Network Map

## Physical Topology
```
Internet
    |
Eero Mesh Device
    |
UDM Pro (192.168.1.1)
    |
SW1 - Catalyst 2960-S (192.168.1.90)
    |
R1 - Cisco 2901 (192.168.1.88)
    |
R2 - Cisco 2911 (192.168.1.101)
    |
SW2 - Catalyst 2960-S (192.168.1.91)

Debian Laptop (192.168.1.75) — connected to SW1
```

## Device Inventory
| Device | Model | IP | Status |
|--------|-------|----|--------|
| UDM Pro | Ubiquiti UDM Pro | 192.168.1.1 | Running |
| SW1 | Catalyst 2960-S PoE+ | 192.168.1.90 | Configured |
| SW2 | Catalyst 2960-S PoE+ | 192.168.1.91 | Configured |
| R1 | Cisco 2901/K9 | 192.168.1.88 | Configured |
| R2 | Cisco 2911/K9 | 192.168.1.101 | IOS installed, not configured |
| Debian | Laptop | 192.168.1.75 | Main workstation |

## VLAN Design
| VLAN | Name | Subnet | Gateway | Purpose |
|------|------|--------|---------|---------|
| 1 | Management | 192.168.1.0/24 | 192.168.1.1 | Device management |
| 10 | MAIN | 10.10.10.0/24 | 10.10.10.1 | Trusted devices |
| 20 | IOT | 10.10.20.0/24 | 10.10.20.1 | Smart TVs, IoT |

## Console Access
- Cable: USB to RJ45 rollover
- Command: `screen /dev/ttyUSB0 9600` (run as root)
- Use working USB port on laptop

## SSH Access
```
ssh -oKexAlgorithms=+diffie-hellman-group14-sha1 -oHostKeyAlgorithms=+ssh-rsa admin@[IP]
Password: cisco123
```
