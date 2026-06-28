# Lab 01 — Network Topology & IP Planning

**Difficulty:** Beginner | **Time:** 1–2 hours
**CCNA Domain:** Network Fundamentals (20%)
**Enterprise Angle:** Every network project starts with a design doc. Corewell Health, Steelcase — nobody racks gear without a plan first.

---

## What You're Building

A network that looks like a small enterprise campus:

```
[Internet]
    |
[Dream Machine Pro]  <-- Your firewall/gateway (like a Cisco ASA at a company)
    |
   R1 (2900) -------- R2 (2900)   <-- Two routers, like two buildings
    |                     |
  SW1 (2960-S)         SW2 (2960-S)  <-- Switches on each floor
```

This is the foundation every other lab builds on. Don't skip it.

---

## Concepts to Understand Before You Start

### What is an IP Address?
Think of it like a mailing address. Every device on a network needs one to send and receive data. Example: `192.168.1.1`

### What is a Subnet?
A subnet groups IP addresses together. `192.168.1.0/24` means addresses `192.168.1.1` through `192.168.1.254` are all on the same network. The `/24` means 24 bits are the "network part."

### Why Point-to-Point Links?
When two routers connect directly to each other (R1 to R2), that cable gets its OWN subnet — `192.168.12.0/30`. This is enterprise standard because it keeps router-to-router traffic isolated.

### What is a VLAN?
VLANs (Virtual LANs) let one physical switch act like multiple separate switches. Corewell Health uses this to keep patient data traffic away from the guest Wi-Fi on the same switch hardware.

---

## Your IP Addressing Scheme

| Device | Interface | IP Address | Purpose |
|--------|-----------|------------|---------|
| Dream Machine Pro | WAN | DHCP from ISP | Internet |
| Dream Machine Pro | LAN | 192.168.1.1 | Default gateway |
| R1 (2900) | G0/0 | 192.168.1.2 | Connects to DMP LAN |
| R1 (2900) | G0/1 | 192.168.12.1 | Connects to R2 |
| R2 (2900) | G0/0 | 192.168.12.2 | Connects to R1 |
| R2 (2900) | G0/1 | 192.168.3.1 | Connects to SW2 side |
| SW1 (2960-S) | VLAN 1 | 192.168.1.3 | Management IP |
| SW2 (2960-S) | VLAN 1 | 192.168.3.2 | Management IP |
| Debian Laptop | eth0 | 192.168.1.10 | Your management machine |

### VLANs Planned for SW1 and SW2
| VLAN | Name | Purpose |
|------|------|---------|
| VLAN 1 | Default | Management (temporary — enterprises move mgmt off VLAN 1 eventually) |
| VLAN 10 | Management | Where network devices get managed from |
| VLAN 20 | Servers | Lab VMs, future servers |
| VLAN 99 | Native | Trunk native VLAN (security best practice) |

---

## Physical Cabling Plan

```
DMP LAN Port  --[ethernet]--> SW1 Port (Fa0/1)
SW1 Uplink    --[ethernet]--> R1 G0/0
R1 G0/1       --[ethernet]--> R2 G0/0   (crossover or auto-MDIX)
R2 G0/1       --[ethernet]--> SW2 Port (Fa0/1)
Laptop        --[ethernet]--> SW1 Port (Fa0/24) or any open port
Console Cable --[USB/Serial]--> R1, R2, SW1, SW2 (for configuration)
```

**You'll need:**
- Straight-through Ethernet cables (blue)
- A console cable (rollover/RJ-45 to USB) — critical for setup
- USB-to-Serial adapter if your laptop has no DB9 port

---

## Lab Tasks

### Task 1 — Draw Your Topology
Before touching any gear, draw the diagram above on paper or in draw.io. Label every interface and IP address.

This is exactly what a network engineer does before a deployment.

### Task 2 — Identify Your Physical Interfaces
On each device, identify the physical ports:
- **2900 routers:** Look for `GigabitEthernet0/0`, `GigabitEthernet0/1`, and a `Console` port
- **2960-S switches:** Look for `FastEthernet0/1` through `0/24` and an `Uplink` port
- **Dream Machine Pro:** WAN port and LAN ports are labeled on the unit

### Task 3 — Cable the Network
Follow the cabling plan above. Don't power anything on yet.
- Label both ends of each cable with tape and a marker
- This is standard practice — you'll thank yourself when troubleshooting

### Task 4 — Document Everything
Fill in this table for your actual hardware (model numbers, serial numbers):

| Device | Role | MAC Address | Notes |
|--------|------|-------------|-------|
| Dream Machine Pro | Firewall/GW | | |
| Router 1 (2900) | R1 | | |
| Router 2 (2900) | R2 | | |
| Switch 1 (2960-S) | SW1 | | |
| Switch 2 (2960-S) | SW2 | | |
| Debian Laptop | Management | | |

---

## Verification Checklist
- [ ] Topology drawn and matches physical cabling
- [ ] Every cable labeled on both ends
- [ ] IP addressing scheme makes sense (no overlaps)
- [ ] VLANs planned and named
- [ ] Hardware documented

---

## What You Learned
- IP addresses identify devices; subnets group them
- Point-to-point links between routers get their own subnet
- VLANs segment traffic on shared switch hardware
- Good documentation is half the job in enterprise networking

---

## Post to Your Website
Your writeup should include:
1. Your topology diagram
2. The IP table
3. One paragraph: "Why does this look like an enterprise network?"

Answer: Because it uses dedicated gateway, routed links between buildings (routers), and switch-level segmentation (VLANs) — exactly what Steelcase or Corewell Health runs across their campuses.

---
*Next: Lab 02 — Console Access & Basic Device Navigation*
