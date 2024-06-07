# leaf-3a

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [DNS Domain](#dns-domain)
  - [IP Name Servers](#ip-name-servers)
  - [Management API HTTP](#management-api-http)
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
| Management0 | oob_management | oob | default | 192.168.0.17/24 | 192.168.0.1 |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management0 | oob_management | oob | default | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management0
   description oob_management
   no shutdown
   ip address 192.168.0.17/24
```

### DNS Domain

DNS domain: atd.lab

#### DNS Domain Device Configuration

```eos
dns domain atd.lab
!
```

### IP Name Servers

#### IP Name Servers Summary

| Name Server | VRF | Priority |
| ----------- | --- | -------- |
| 192.168.2.1 | default | - |
| 8.8.8.8 | default | - |

#### IP Name Servers Device Configuration

```eos
ip name-server vrf default 8.8.8.8
ip name-server vrf default 192.168.2.1
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| default | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf default
      no shutdown
```

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| IDF3_AGG | Vlan4094 | 10.255.255.11 | Port-Channel47 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id IDF3_AGG
   local-interface Vlan4094
   peer-address 10.255.255.11
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
| 110 | IDF1-Data | - |
| 210 | IDF2-Data | - |
| 4094 | MLAG_PEER | MLAG |

### VLANs Device Configuration

```eos
!
vlan 110
   name IDF1-Data
!
vlan 210
   name IDF2-Data
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
| Ethernet47 | MLAG_PEER_leaf-3b_Ethernet47 | *trunk | *- | *- | *['MLAG'] | 47 |
| Ethernet48 | MLAG_PEER_leaf-3b_Ethernet48 | *trunk | *- | *- | *['MLAG'] | 47 |
| Ethernet49 | SPINE-1_Ethernet5 | *trunk | *110,210 | *- | *- | 49 |
| Ethernet50 | SPINE-2_Ethernet5 | *trunk | *110,210 | *- | *- | 49 |
| Ethernet51/1 | MEMBER-LEAF-3C_Ethernet49 | *trunk | *110,210 | *- | *- | 511 |
| Ethernet52/1 | MEMBER-LEAF-3D_Ethernet49 | *trunk | *110,210 | *- | *- | 521 |
| Ethernet53/1 | MEMBER-LEAF-3E_Ethernet49 | *trunk | *110,210 | *- | *- | 531 |

*Inherited from Port-Channel Interface

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet47
   description MLAG_PEER_leaf-3b_Ethernet47
   no shutdown
   channel-group 47 mode active
!
interface Ethernet48
   description MLAG_PEER_leaf-3b_Ethernet48
   no shutdown
   channel-group 47 mode active
!
interface Ethernet49
   description SPINE-1_Ethernet5
   no shutdown
   channel-group 49 mode active
!
interface Ethernet50
   description SPINE-2_Ethernet5
   no shutdown
   channel-group 49 mode active
!
interface Ethernet51/1
   description MEMBER-LEAF-3C_Ethernet49
   no shutdown
   channel-group 511 mode active
!
interface Ethernet52/1
   description MEMBER-LEAF-3D_Ethernet49
   no shutdown
   channel-group 521 mode active
!
interface Ethernet53/1
   description MEMBER-LEAF-3E_Ethernet49
   no shutdown
   channel-group 531 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel47 | MLAG_PEER_leaf-3b_Po47 | switched | trunk | - | - | ['MLAG'] | - | - | - | - |
| Port-Channel49 | CAMPUS_SPINES_Po5 | switched | trunk | 110,210 | - | - | - | - | 49 | - |
| Port-Channel511 | MEMBER-LEAF-3C_Po49 | switched | trunk | 110,210 | - | - | - | - | 511 | - |
| Port-Channel521 | MEMBER-LEAF-3D_Po49 | switched | trunk | 110,210 | - | - | - | - | 521 | - |
| Port-Channel531 | MEMBER-LEAF-3E_Po49 | switched | trunk | 110,210 | - | - | - | - | 531 | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel47
   description MLAG_PEER_leaf-3b_Po47
   no shutdown
   switchport
   switchport mode trunk
   switchport trunk group MLAG
!
interface Port-Channel49
   description CAMPUS_SPINES_Po5
   no shutdown
   switchport
   switchport trunk allowed vlan 110,210
   switchport mode trunk
   mlag 49
!
interface Port-Channel511
   description MEMBER-LEAF-3C_Po49
   no shutdown
   switchport
   switchport trunk allowed vlan 110,210
   switchport mode trunk
   mlag 511
!
interface Port-Channel521
   description MEMBER-LEAF-3D_Po49
   no shutdown
   switchport
   switchport trunk allowed vlan 110,210
   switchport mode trunk
   mlag 521
!
interface Port-Channel531
   description MEMBER-LEAF-3E_Po49
   no shutdown
   switchport
   switchport trunk allowed vlan 110,210
   switchport mode trunk
   mlag 531
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan4094 | MLAG_PEER | default | 9214 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan4094 |  default  |  10.255.255.10/31  |  -  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan4094
   description MLAG_PEER
   no shutdown
   mtu 9214
   no autostate
   ip address 10.255.255.10/31
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

#### IP Routing Device Configuration

```eos
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| default | false |

### Static Routes

#### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP | Exit interface | Administrative Distance | Tag | Route Name | Metric |
| --- | ------------------ | ----------- | -------------- | ----------------------- | --- | ---------- | ------ |
| default | 0.0.0.0/0 | 192.168.0.1 | - | 1 | - | - | - |

#### Static Routes Device Configuration

```eos
!
ip route 0.0.0.0/0 192.168.0.1
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

### VRF Instances Device Configuration

```eos
```
