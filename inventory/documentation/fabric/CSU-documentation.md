# CSU

## Table of Contents

- [Fabric Switches and Management IP](#fabric-switches-and-management-ip)
  - [Fabric Switches with inband Management IP](#fabric-switches-with-inband-management-ip)
- [Fabric Topology](#fabric-topology)
- [Fabric IP Allocation](#fabric-ip-allocation)
  - [Fabric Point-To-Point Links](#fabric-point-to-point-links)
  - [Point-To-Point Links Node Allocation](#point-to-point-links-node-allocation)
  - [Loopback Interfaces (BGP EVPN Peering)](#loopback-interfaces-bgp-evpn-peering)
  - [Loopback0 Interfaces Node Allocation](#loopback0-interfaces-node-allocation)
  - [VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)](#vtep-loopback-vxlan-tunnel-source-interfaces-vteps-only)
  - [VTEP Loopback Node allocation](#vtep-loopback-node-allocation)

## Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision | Serial Number |
| --- | ---- | ---- | ------------- | -------- | -------------------------- | ------------- |
| CSU | l2leaf | camsw1 | 172.16.100.105/24 | - | Not Available | - |
| CSU | l2leaf | camsw2 | 172.16.100.106/24 | - | Not Available | - |
| CSU | l3leaf | leaf1a-dc1 | 192.168.3.2/25 | cEOSLab | Not Available | - |
| CSU | l3leaf | leaf1b-dc1 | 192.168.3.19/25 | cEOSLab | Not Available | - |
| CSU | l3leaf | leaf2a-dc1 | 192.168.3.3/25 | cEOSLab | Not Available | - |
| CSU | l3leaf | leaf2b-dc1 | 192.168.3.17/25 | cEOSLab | Not Available | - |
| CSU | l3leaf | leaf3a-dc1 | 192.168.3.4/25 | cEOSLab | Not Available | - |
| CSU | l3leaf | leaf3b-dc1 | 192.168.3.18/25 | cEOSLab | Not Available | - |
| CSU | spine | spine1-dc1 | 192.168.3.5/24 | cEOSLab | Not Available | - |
| CSU | spine | spine2-dc1 | 192.168.3.6/24 | cEOSLab | Not Available | - |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l2leaf | camsw1 | Ethernet47 | mlag_peer | camsw2 | Ethernet47 |
| l2leaf | camsw1 | Ethernet48 | mlag_peer | camsw2 | Ethernet48 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 10.255.1.0/24 | 256 | 0 | 0.0 % |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 10.0.1.0/24 | 256 | 0 | 0.0 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| --------------------- | ------------------- | ------------------ | ------------------ |
| 10.1.1.0/24 | 256 | 0 | 0.0 % |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
