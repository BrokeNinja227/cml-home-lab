# Phase 2: DHCP and static addressing

## Goal


## Topology
![Topology](screenshots/topology.png)

**Devices:**

**Links:**


## IP Plan
| VLAN / Network | Subnet | Gateway | Addressing method |
|---|---|---|---|
| [10 - Sales] | [192.168.10.0/24] | [.1] | [Static] |
| [20 - IT] | [192.168.20.0/24] | [.1] | [Static] |

## What I Configured


Full device configs are in [`configs/`](configs/).

## Verification

R1#show ip dhcp binding
Bindings from all pools not associated with VRF:
IP address          Client-ID/              Lease expiration        Type
                    Hardware address/
                    User name
192.168.10.100      ff32.39f9.b500.0200.    Nov 02 2026 11:44 PM    Automatic
                    00ab.1176.a76d.21ee.
                    2033.f7

SW1#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol
GigabitEthernet0/0     unassigned      YES unset  up                    up      
GigabitEthernet0/1     unassigned      YES unset  up                    up      
GigabitEthernet0/2     unassigned      YES unset  up                    up      
GigabitEthernet0/3     unassigned      YES unset  down                  down    
Vlan99                 192.168.99.2    YES manual up                    up 

cisco@inserthostname-here:~$ ping -c 4 192.168.10.1
PING 192.168.10.1 (192.168.10.1) 56(84) bytes of data.
64 bytes from 192.168.10.1: icmp_seq=1 ttl=255 time=9.65 ms
64 bytes from 192.168.10.1: icmp_seq=2 ttl=255 time=4.81 ms
64 bytes from 192.168.10.1: icmp_seq=3 ttl=255 time=4.55 ms
64 bytes from 192.168.10.1: icmp_seq=4 ttl=255 time=7.92 ms

--- 192.168.10.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3007ms
rtt min/avg/max/mdev = 4.545/6.729/9.649/2.145 ms

cisco@inserthostname-here:~$ ping -c 4 192.168.20.10
PING 192.168.20.10 (192.168.20.10) 56(84) bytes of data.
64 bytes from 192.168.20.10: icmp_seq=1 ttl=63 time=12.9 ms
64 bytes from 192.168.20.10: icmp_seq=2 ttl=63 time=22.0 ms
64 bytes from 192.168.20.10: icmp_seq=3 ttl=63 time=7.25 ms
64 bytes from 192.168.20.10: icmp_seq=4 ttl=63 time=5.75 ms

--- 192.168.20.10 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3007ms
rtt min/avg/max/mdev = 5.748/11.960/21.965/6.357 ms

## Troubleshooting


## What I Learned


## Files
- [intervlanrouting.yaml](): exported CML lab; import it to reproduce this topology
- [`configs/`](configs/): running configs for each device
- [`screenshots/`](screenshots/): topology and verification images

