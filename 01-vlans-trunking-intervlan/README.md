# Phase 1: VLANs, trunking, and inter-VLAN routing (router-on-a-stick)

## Goal
Use virtual machines to replicate inter-VLAN routing, allowing network traffic to travel across separate IP subnets.

## Topology
<img width="842" height="497" alt="Screenshot 2026-10-26 110524" src="https://github.com/user-attachments/assets/61b14e96-3557-4448-9cca-04958ee3b3f2" />

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

```
PC1$ ping -c 4 192.168.20.10
PING 192.168.20.10 (192.168.20.10) 56(84) bytes of data.
64 bytes from 192.168.20.10: icmp_seq=1 ttl=63 time=67.1 ms
64 bytes from 192.168.20.10: icmp_seq=2 ttl=63 time=21.7 ms
64 bytes from 192.168.20.10: icmp_seq=3 ttl=63 time=17.0 ms
64 bytes from 192.168.20.10: icmp_seq=4 ttl=63 time=13.5 ms

--- 192.168.20.10 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3016ms
rtt min/avg/max/mdev = 13.457/29.806/67.101/21.728 ms
```

```
PC2$ ping -c 4 192.168.10.10
PING 192.168.10.10 (192.168.10.10) 56(84) bytes of data.
64 bytes from 192.168.10.10: icmp_seq=1 ttl=63 time=14.9 ms
64 bytes from 192.168.10.10: icmp_seq=2 ttl=63 time=25.7 ms
64 bytes from 192.168.10.10: icmp_seq=3 ttl=63 time=17.4 ms
64 bytes from 192.168.10.10: icmp_seq=4 ttl=63 time=12.1 ms

--- 192.168.10.10 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3016ms
rtt min/avg/max/mdev = 12.141/17.527/25.690/5.065 ms
```

```
SW1# show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    Gi0/3
10   Sales                            active    Gi0/1
20   IT                               active    Gi0/2
1002 fddi-default                     act/unsup 
1003 token-ring-default               act/unsup 
1004 fddinet-default                  act/unsup 
1005 trnet-default                    act/unsup 
```

```
R1# show ip interface brief
Interface                  IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0         unassigned      YES NVRAM  up                    up      
GigabitEthernet0/0.10      192.168.10.1    YES NVRAM  up                    up      
GigabitEthernet0/0.20      192.168.20.1    YES NVRAM  up                    up      
GigabitEthernet0/1         unassigned      YES NVRAM  administratively down down    
GigabitEthernet0/2         unassigned      YES NVRAM  administratively down down    
GigabitEthernet0/3         unassigned      YES NVRAM  administratively down down
```

## Troubleshooting
	Symptom - “Network is unreachable”
	Cause - non-persistent host settings after a restart
	Solution - Reapply Host settings copy running-config startup-config

## What I Learned
VLANs isolate network traffic by default. Inter-VLAN routing allows separate virtual networks to communicate at layer 3.

## Files
- `lab.yaml`: exported CML lab; import it to reproduce this topology
- `configs/`: running configs for each device
- `screenshots/`: topology and verification images

