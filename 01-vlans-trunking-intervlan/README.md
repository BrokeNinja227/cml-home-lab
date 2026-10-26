# Phase 1: VLANs, trunking, and inter-VLAN routing (router-on-a-stick)

## Goal
Use virtual machines to replicate inter-VLAN routing, allowing network traffic to travel across separate IP subnets.

## Topology
![Topology](screenshots/topology.png) <img width="842" height="497" alt="Screenshot 2026-10-26 110524" src="https://github.com/user-attachments/assets/61b14e96-3557-4448-9cca-04958ee3b3f2" />

Devices: 1 router (IOSv), 1 switch (IOSvL2), 2 Ubuntu hosts

## IP Plan
| VLAN / Network | Subnet | Gateway | Addressing method |
|---|---|---|---|
| [10 - Sales] | [192.168.10.0/24] | [.1] | [Static] |
| [20 - IT] | [192.168.20.0/24] | [.1] | [Static] |

## What I Configured
- Created VLANs 10 and 20 on SW1
- Configured router-on-a-stick with subinterfaces g0/0.10 and g0/0.20

Full device configs are in [`configs/`](configs/).

## Verification
[Show proof it works: paste key command output or a screenshot.]

```
PC1$ ping -c 4 192.168.20.10
[paste output]
```

```
SW1# show vlan brief
[paste output]
```

## Troubleshooting
	N/A

## What I Learned
VLANs isolate network traffic by default. Inter-VLAN routing allows separate virtual networks to communicate at layer 3.

## Files
- `lab.yaml`: exported CML lab; import it to reproduce this topology
- `configs/`: running configs for each device
- `screenshots/`: topology and verification images

