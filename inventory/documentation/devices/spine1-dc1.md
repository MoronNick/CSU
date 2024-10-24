# spine1-dc1

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [IP Name Servers](#ip-name-servers)
  - [Clock Settings](#clock-settings)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [TACACS Servers](#tacacs-servers)
  - [AAA Authentication](#aaa-authentication)
- [Monitoring](#monitoring)
  - [Logging](#logging)
  - [SNMP](#snmp)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management0 | oob_management | oob | MGMT | 192.168.3.5/24 | 192.168.3.1 |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management0 | oob_management | oob | MGMT | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management0
   description oob_management
   no shutdown
   vrf MGMT
   ip address 192.168.3.5/24
```

### IP Name Servers

#### IP Name Servers Summary

| Name Server | VRF | Priority |
| ----------- | --- | -------- |
| 8.8.8.8 | MGMT | - |
| 208.67.222.222 | MGMT | - |

#### IP Name Servers Device Configuration

```eos
ip name-server vrf MGMT 8.8.8.8
ip name-server vrf MGMT 208.67.222.222
```

### Clock Settings

#### Clock Timezone Settings

Clock Timezone is set to **MST**.

#### Clock Device Configuration

```eos
!
clock timezone MST
```

### NTP

#### NTP Summary

##### NTP Local Interface

| Interface | VRF |
| --------- | --- |
| Management0 | MGMT |

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 192.168.1.6 | MGMT | True | - | - | - | - | - | - | - |

#### NTP Device Configuration

```eos
!
ntp local-interface vrf MGMT Management0
ntp server vrf MGMT 192.168.1.6 prefer
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| MGMT | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf MGMT
      no shutdown
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| admin | 15 | network-admin | False | - |
| ansible | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin nopassword
username ansible privilege 15 role network-admin secret sha512 <removed>
```

### TACACS Servers

#### TACACS Servers

| VRF | TACACS Servers | Single-Connection | Timeout |
| --- | -------------- | ----------------- | ------- |
| default | 1.1.1.1 | False | - |

#### TACACS Servers Device Configuration

```eos
!
tacacs-server host 1.1.1.1 key 7 <removed>
```

### AAA Authentication

#### AAA Authentication Summary

| Type | Sub-type | User Stores |
| ---- | -------- | ---------- |
| Login | default | group tacacs+ local |
| Login | console | group tacacs+ local |

#### AAA Authentication Device Configuration

```eos
aaa authentication login default group tacacs+ local
aaa authentication login console group tacacs+ local
!
```

## Monitoring

### Logging

#### Logging Servers and Features Summary

| Type | Level |
| -----| ----- |

| VRF | Source Interface |
| --- | ---------------- |
| default | Management1 |

| VRF | Hosts | Ports | Protocol |
| --- | ----- | ----- | -------- |
| default | 10.100.201.199 | Default | UDP |

#### Logging Servers and Features Device Configuration

```eos
!
logging host 10.100.201.199
logging source-interface Management1
```

### SNMP

#### SNMP Configuration Summary

| Contact | Location | SNMP Traps | State |
| ------- | -------- | ---------- | ----- |
| noc@colostate.edu | E7 row 5 cab 5 | All | Disabled |

#### SNMP Hosts Configuration

| Host | VRF | Community | Username | Authentication level | SNMP Version |
| ---- |---- | --------- | -------- | -------------------- | ------------ |
| 10.100.201.190 | default | <removed> | - | - | 2c |

#### SNMP Communities

| Community | Access | Access List IPv4 | Access List IPv6 | View |
| --------- | ------ | ---------------- | ---------------- | ---- |
| <removed> | ro | - | - | - |
| <removed> | rw | - | - | - |

#### SNMP Device Configuration

```eos
!
snmp-server contact noc@colostate.edu
snmp-server location E7 row 5 cab 5
snmp-server community <removed> ro
snmp-server community <removed> rw
snmp-server host 10.100.201.190 vrf default version 2c <removed>
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **none**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode none
```

## Internal VLAN Allocation Policy

### Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

### Internal VLAN Allocation Policy Device Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet1 | P2P_LINK_TO_LEAF1A-DC1_Ethernet1 | routed | - | 10.255.1.0/31 | default | 1500 | False | - | - |
| Ethernet2 | P2P_LINK_TO_LEAF1B-DC1_Ethernet1 | routed | - | 10.255.1.4/31 | default | 1500 | False | - | - |
| Ethernet3 | P2P_LINK_TO_LEAF2A-DC1_Ethernet1 | routed | - | 10.255.1.8/31 | default | 1500 | False | - | - |
| Ethernet4 | P2P_LINK_TO_LEAF2B-DC1_Ethernet1 | routed | - | 10.255.1.12/31 | default | 1500 | False | - | - |
| Ethernet5 | P2P_LINK_TO_LEAF3A-DC1_Ethernet1 | routed | - | 10.255.1.16/31 | default | 1500 | False | - | - |
| Ethernet6 | P2P_LINK_TO_LEAF3B-DC1_Ethernet1 | routed | - | 10.255.1.20/31 | default | 1500 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description P2P_LINK_TO_LEAF1A-DC1_Ethernet1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.1.0/31
!
interface Ethernet2
   description P2P_LINK_TO_LEAF1B-DC1_Ethernet1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.1.4/31
!
interface Ethernet3
   description P2P_LINK_TO_LEAF2A-DC1_Ethernet1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.1.8/31
!
interface Ethernet4
   description P2P_LINK_TO_LEAF2B-DC1_Ethernet1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.1.12/31
!
interface Ethernet5
   description P2P_LINK_TO_LEAF3A-DC1_Ethernet1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.1.16/31
!
interface Ethernet6
   description P2P_LINK_TO_LEAF3B-DC1_Ethernet1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.255.1.20/31
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 10.0.1.25/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 10.0.1.25/32
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| MGMT | False |

#### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf MGMT
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| MGMT | false |

### Static Routes

#### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP | Exit interface | Administrative Distance | Tag | Route Name | Metric |
| --- | ------------------ | ----------- | -------------- | ----------------------- | --- | ---------- | ------ |
| MGMT | 0.0.0.0/0 | 192.168.3.1 | - | 1 | - | - | - |

#### Static Routes Device Configuration

```eos
!
ip route vrf MGMT 0.0.0.0/0 192.168.3.1
```

### Router BGP

ASN Notation: asplain

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65100 | 10.0.1.25 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| maximum-paths 4 ecmp 4 |

#### Router BGP Peer Groups

##### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Next-hop unchanged | True |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

##### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 10.0.1.4 | 65101 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.0.1.5 | 65101 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.0.1.6 | 65102 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.0.1.7 | 65102 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.0.1.8 | 65103 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.0.1.9 | 65103 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.255.1.1 | 65101 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.255.1.5 | 65101 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.255.1.9 | 65102 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.255.1.13 | 65102 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.255.1.17 | 65103 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.255.1.21 | 65103 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Encapsulation |
| ---------- | -------- | ------------- |
| EVPN-OVERLAY-PEERS | True | default |

#### Router BGP Device Configuration

```eos
!
router bgp 65100
   router-id 10.0.1.25
   maximum-paths 4 ecmp 4
   no bgp default ipv4-unicast
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS next-hop-unchanged
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor 10.0.1.4 peer group EVPN-OVERLAY-PEERS
   neighbor 10.0.1.4 remote-as 65101
   neighbor 10.0.1.4 description leaf1a-dc1
   neighbor 10.0.1.5 peer group EVPN-OVERLAY-PEERS
   neighbor 10.0.1.5 remote-as 65101
   neighbor 10.0.1.5 description leaf1b-dc1
   neighbor 10.0.1.6 peer group EVPN-OVERLAY-PEERS
   neighbor 10.0.1.6 remote-as 65102
   neighbor 10.0.1.6 description leaf2a-dc1
   neighbor 10.0.1.7 peer group EVPN-OVERLAY-PEERS
   neighbor 10.0.1.7 remote-as 65102
   neighbor 10.0.1.7 description leaf2b-dc1
   neighbor 10.0.1.8 peer group EVPN-OVERLAY-PEERS
   neighbor 10.0.1.8 remote-as 65103
   neighbor 10.0.1.8 description leaf3a-dc1
   neighbor 10.0.1.9 peer group EVPN-OVERLAY-PEERS
   neighbor 10.0.1.9 remote-as 65103
   neighbor 10.0.1.9 description leaf3b-dc1
   neighbor 10.255.1.1 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.1.1 remote-as 65101
   neighbor 10.255.1.1 description leaf1a-dc1_Ethernet1
   neighbor 10.255.1.5 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.1.5 remote-as 65101
   neighbor 10.255.1.5 description leaf1b-dc1_Ethernet1
   neighbor 10.255.1.9 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.1.9 remote-as 65102
   neighbor 10.255.1.9 description leaf2a-dc1_Ethernet1
   neighbor 10.255.1.13 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.1.13 remote-as 65102
   neighbor 10.255.1.13 description leaf2b-dc1_Ethernet1
   neighbor 10.255.1.17 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.1.17 remote-as 65103
   neighbor 10.255.1.17 description leaf3a-dc1_Ethernet1
   neighbor 10.255.1.21 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.255.1.21 remote-as 65103
   neighbor 10.255.1.21 description leaf3b-dc1_Ethernet1
   redistribute connected route-map RM-CONN-2-BGP
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
```

## BFD

### Router BFD

#### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 300 | 300 | 3 |

#### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 300 min-rx 300 multiplier 3
```

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.0.1.0/24 eq 32 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.0.1.0/24 eq 32
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| MGMT | disabled |

### VRF Instances Device Configuration

```eos
!
vrf instance MGMT
```
