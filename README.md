# SSH Network Security Lab — Cisco Packet Tracer

A multi-router network lab focused on SSH hardening, VLAN segmentation, 
serial WAN connectivity, and ACL-based management plane isolation.

---

## Topology

---

## IP Addressing

| Device | Interface | IP | Subnet |
|--------|-----------|-----|--------|
| R1 | Gig0/0.10 | 192.168.10.1 | /24 |
| R1 | Gig0/0.20 | 192.168.20.1 | /24 |
| R1 | Se0/3/0 | 10.0.0.1 | /30 |
| R2 | Gig0/0 | 192.168.30.1 | /24 |
| R2 | Se0/3/0 | 10.0.0.2 | /30 |
| Admin-PC | NIC | 192.168.20.10 | /24 |
| User-PC | NIC | 192.168.30.10 | /24 |

---

## Features Implemented

- VLAN 10 and VLAN 20 on SW1 with Router-on-a-Stick on R1
- Serial WAN link between R1 (DCE) and R2 (DTE)
- Static routing between HQ and Branch networks
- SSH v2 hardened on both routers
- Extended ACL blocking User-PC from SSH while permitting Admin-PC only

---

## ACL Logic

- Admin-PC (192.168.20.10) → SSH permitted to both routers
- User-PC (192.168.30.0/24) → SSH denied on both routers
- All other traffic → permitted

---

## SSH Hardening Config (R1)

ip domain-name benzi.local
crypto key generate rsa general-keys modulus 2048
username admin secret [password]
ip ssh version 2
line vty 0 4
transport input ssh
login local

---

## ACL Config

ip access-list extended BLOCK_SSH
permit tcp host 192.168.20.10 any eq 22
deny tcp 192.168.30.0 0.0.0.255 any eq 22
permit ip any any

Applied inbound on R1 Gig0/0.10 and R2 Gig0/0

---

## Author

Jawad Talat — BS Cybersecurity Student | CCNA 
