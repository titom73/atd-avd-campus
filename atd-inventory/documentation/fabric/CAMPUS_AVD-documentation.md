# CAMPUS_AVD

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
| CAMPUS_AVD | leaf | leaf-1a | {{ hostvars["leaf-1a"].ansible_host }}/24 | cEOSLab | Provisioned | - |
| CAMPUS_AVD | leaf | leaf-1b | {{ hostvars["leaf-1b"].ansible_host }}/24 | cEOSLab | Provisioned | - |
| CAMPUS_AVD | leaf | leaf-2a | {{ hostvars["leaf-2a"].ansible_host }}/24 | 720XP | Provisioned | - |
| CAMPUS_AVD | leaf | leaf-3a | 192.168.0.17/24 | cEOSLab | Provisioned | - |
| CAMPUS_AVD | leaf | leaf-3b | 192.168.0.18/24 | cEOSLab | Provisioned | - |
| CAMPUS_AVD | leaf | member-leaf-3c | 192.168.0.19/24 | cEOSLab | Provisioned | - |
| CAMPUS_AVD | leaf | member-leaf-3d | 192.168.0.20/24 | cEOSLab | Provisioned | - |
| CAMPUS_AVD | leaf | member-leaf-3e | 192.168.0.21/24 | cEOSLab | Provisioned | - |
| CAMPUS_AVD | l3spine | spine-1 | {{ hostvars["spine-1"].ansible_host }}/24 | cEOSLab | Provisioned | - |
| CAMPUS_AVD | l3spine | spine-2 | {{ hostvars["spine-2"].ansible_host }}/24 | cEOSLab | Provisioned | - |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| leaf | leaf-1a | Ethernet47 | mlag_peer | leaf-1b | Ethernet47 |
| leaf | leaf-1a | Ethernet48 | mlag_peer | leaf-1b | Ethernet48 |
| leaf | leaf-1a | Ethernet49 | l3spine | spine-1 | Ethernet3 |
| leaf | leaf-1b | Ethernet49 | l3spine | spine-2 | Ethernet3 |
| leaf | leaf-2a | Ethernet1/1 | l3spine | spine-1 | Ethernet4 |
| leaf | leaf-2a | Ethernet2/1 | l3spine | spine-2 | Ethernet4 |
| leaf | leaf-3a | Ethernet47 | mlag_peer | leaf-3b | Ethernet47 |
| leaf | leaf-3a | Ethernet48 | mlag_peer | leaf-3b | Ethernet48 |
| leaf | leaf-3a | Ethernet49 | l3spine | spine-1 | Ethernet5 |
| leaf | leaf-3a | Ethernet50 | l3spine | spine-2 | Ethernet5 |
| leaf | leaf-3a | Ethernet51/1 | leaf | member-leaf-3c | Ethernet49 |
| leaf | leaf-3a | Ethernet52/1 | leaf | member-leaf-3d | Ethernet49 |
| leaf | leaf-3a | Ethernet53/1 | leaf | member-leaf-3e | Ethernet49 |
| leaf | leaf-3b | Ethernet49 | l3spine | spine-1 | Ethernet6 |
| leaf | leaf-3b | Ethernet50 | l3spine | spine-2 | Ethernet6 |
| leaf | leaf-3b | Ethernet51/1 | leaf | member-leaf-3c | Ethernet50 |
| leaf | leaf-3b | Ethernet52/1 | leaf | member-leaf-3d | Ethernet50 |
| leaf | leaf-3b | Ethernet53/1 | leaf | member-leaf-3e | Ethernet50 |
| l3spine | spine-1 | Ethernet49 | mlag_peer | spine-2 | Ethernet49 |
| l3spine | spine-1 | Ethernet50 | mlag_peer | spine-2 | Ethernet50 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 172.16.1.0/24 | 256 | 2 | 0.79 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| CAMPUS_AVD | spine-1 | 172.16.1.1/32 |
| CAMPUS_AVD | spine-2 | 172.16.1.2/32 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| --------------------- | ------------------- | ------------------ | ------------------ |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
