# camsw1

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
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management0 | oob_management | oob | MGMT | 172.16.100.105/24 | 192.168.3.1 |

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
   ip address 172.16.100.105/24
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

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| MLAG | Vlan4094 | 192.168.0.59 | Port-Channel47 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id MLAG
   local-interface Vlan4094
   peer-address 192.168.0.59
   peer-link Port-Channel47
   reload-delay mlag 300
   reload-delay non-mlag 330
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 16384 |

#### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4094
spanning-tree mst 0 priority 16384
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

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 11 | ACNS_Mgmt | - |
| 103 | ACNS_Services_103 | - |
| 408 | Cardkey_DataCenter | - |
| 553 | Windows_Unix_MGMT | - |
| 592 | Security_Cameras | - |
| 918 | Cluster_comm_10.2.103 | - |
| 920 | VM_Mgmt | - |
| 922 | Vmotion | - |
| 1000 | san_0 | - |
| 1001 | san_1 | - |
| 4094 | MLAG_PEER | MLAG |

### VLANs Device Configuration

```eos
!
vlan 11
   name ACNS_Mgmt
!
vlan 103
   name ACNS_Services_103
!
vlan 408
   name Cardkey_DataCenter
!
vlan 553
   name Windows_Unix_MGMT
!
vlan 592
   name Security_Cameras
!
vlan 918
   name Cluster_comm_10.2.103
!
vlan 920
   name VM_Mgmt
!
vlan 922
   name Vmotion
!
vlan 1000
   name san_0
!
vlan 1001
   name san_1
!
vlan 4094
   name MLAG_PEER
   trunk group MLAG
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet1 |  UNIX_R5C7U1-2_CAMSYSPIVOT1_iDRAC | access | 553 | - | - | - |
| Ethernet2 |  UNIX_R5C7U1-2_CAMSYSPIVOT1_Eth0/1 | access | 1001 | - | - | - |
| Ethernet3 |  UNIX_R5C7U1-2_CAMSYSPIVOT1_Eth4/1 | access | 592 | - | - | - |
| Ethernet5 |  UNIX_R5C7U1-2_CAMSYSPIVOT2_iDRAC | access | 553 | - | - | - |
| Ethernet6 |  UNIX_R5C7U1-2_CAMSYSPIVOT2_Eth0/2 | access | 1001 | - | - | - |
| Ethernet7 |  UNIX_R5C7U1-2_CAMSYSPIVOT2_Eth0/2 | access | 592 | - | - | - |
| Ethernet15 | - | *trunk | *- | *- | *- | 645 |
| Ethernet47 | MLAG_PEER_camsw2_Ethernet47 | *trunk | *- | *- | *['MLAG'] | 47 |
| Ethernet48 | MLAG_PEER_camsw2_Ethernet48 | *trunk | *- | *- | *['MLAG'] | 47 |

*Inherited from Port-Channel Interface

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description UNIX_R5C7U1-2_CAMSYSPIVOT1_iDRAC
   no shutdown
   switchport access vlan 553
   switchport mode access
   switchport
   spanning-tree portfast
   spanning-tree bpduguard enable
!
interface Ethernet2
   description UNIX_R5C7U1-2_CAMSYSPIVOT1_Eth0/1
   no shutdown
   switchport access vlan 1001
   switchport mode access
   switchport
   spanning-tree portfast
!
interface Ethernet3
   description UNIX_R5C7U1-2_CAMSYSPIVOT1_Eth4/1
   no shutdown
   switchport access vlan 592
   switchport mode access
   switchport
   spanning-tree portfast
!
interface Ethernet5
   description UNIX_R5C7U1-2_CAMSYSPIVOT2_iDRAC
   no shutdown
   switchport access vlan 553
   switchport mode access
   switchport
   spanning-tree portfast
!
interface Ethernet6
   description UNIX_R5C7U1-2_CAMSYSPIVOT2_Eth0/2
   no shutdown
   switchport access vlan 1001
   switchport mode access
   switchport
   spanning-tree portfast
!
interface Ethernet7
   description UNIX_R5C7U1-2_CAMSYSPIVOT2_Eth0/2
   no shutdown
   switchport access vlan 592
   switchport mode access
   switchport
   spanning-tree portfast
!
interface Ethernet15
   no shutdown
   channel-group 645 mode active
!
interface Ethernet47
   description MLAG_PEER_camsw2_Ethernet47
   no shutdown
   channel-group 47 mode active
!
interface Ethernet48
   description MLAG_PEER_camsw2_Ethernet48
   no shutdown
   channel-group 47 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel47 | MLAG_PEER_camsw2_Po47 | switched | trunk | - | - | ['MLAG'] | - | - | - | - |
| Port-Channel645 | - | switched | trunk | - | - | - | - | - | 645 | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel47
   description MLAG_PEER_camsw2_Po47
   no shutdown
   switchport
   switchport mode trunk
   switchport trunk group MLAG
!
interface Port-Channel645
   no shutdown
   switchport
   switchport mode trunk
   mlag 645
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan4094 | MLAG_PEER | default | 1500 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan4094 |  default  |  192.168.0.58/31  |  -  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan4094
   description MLAG_PEER
   no shutdown
   mtu 1500
   no autostate
   ip address 192.168.0.58/31
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
| default | False |
| MGMT | False |

#### IP Routing Device Configuration

```eos
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

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Device Configuration

```eos
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
