# leaf-3a

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [DNS Domain](#dns-domain)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [RADIUS Server](#radius-server)
  - [AAA Server Groups](#aaa-server-groups)
  - [AAA Authentication](#aaa-authentication)
  - [AAA Authorization](#aaa-authorization)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Configuration](#internal-vlan-allocation-policy-configuration)
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
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [System L1](#system-l1)
  - [Unsupported interface configurations](#unsupported-interface-configurations)
  - [System L1 Configuration](#system-l1-configuration)
- [EOS CLI](#eos-cli)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management0 | oob_management | oob | default | 192.168.0.17/24 | - |

##### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
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

#### DNS domain: atd.lab

#### DNS Domain Device Configuration

```eos
dns domain atd.lab
!
```

### NTP

#### NTP Summary

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 192.168.0.1 | default | - | - | True | - | - | - | - | - |

#### NTP Device Configuration

```eos
!
ntp server 192.168.0.1 iburst
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

#### Management API HTTP Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf default
      no shutdown
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| arista | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
username arista privilege 15 role network-admin secret sha512 <removed>
username arista ssh-key ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCmmhnaL5nwd6P37UdmIXlc3IdwqGoFSWEVDgrxHK7IQZcBLouVKW2Lec3onmXeRhlOi0MHczrSY8pzQy2EcV0Lsv1y/r+9Agotrb90e5QZyotlKtOiHdxoIRuNUXepI+qPhKiIejordgaaaevfYSPU9qtV0BeDWc+UPeJ2YNEQy9KM5wog7TPJhl52wew6Fgltg4LqsFjeZN5cuifUBbxtbkkuP+tPECD6H6fNDgzqiGAbH0f2e9WMld/LbX+eUvcPPV/kGnmnF98toed6V8E5UiiRVvjSCBrZjojgg31L2uDs79k36gQ3VcRcLRm3TL9LwRb8cYdS1wOlPZqABFDJ arista@christophe-testing-1-bcde41d8-eos.c.beta-atds.internal
```

### RADIUS Server

#### RADIUS Server Hosts

| VRF | RADIUS Servers | Timeout | Retransmit |
| --- | -------------- | ------- | ---------- |
| default | 192.168.0.1 | - | - |

#### RADIUS Server Device Configuration

```eos
!
radius-server host 192.168.0.1 key 7 <removed>
```

### AAA Server Groups

#### AAA Server Groups Summary

| Server Group Name | Type  | VRF | IP address |
| ------------------| ----- | --- | ---------- |
| atds | radius | default | 192.168.0.1 |

#### AAA Server Groups Device Configuration

```eos
!
aaa group server radius atds
   server 192.168.0.1
```

### AAA Authentication

#### AAA Authentication Summary

| Type | Sub-type | User Stores |
| ---- | -------- | ---------- |
| Login | default | group atds local |

#### AAA Authentication Device Configuration

```eos
aaa authentication login default group atds local
!
```

### AAA Authorization

#### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | group atds local |

Authorization for configuration commands is disabled.

#### AAA Authorization Privilege Levels Summary

| Privilege Level | User Stores |
| --------------- | ----------- |
| all | local |

#### AAA Authorization Device Configuration

```eos
aaa authorization exec default group atds local
aaa authorization commands all default local
!
```

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 192.168.0.5:9910 | default | token,/tmp/token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=192.168.0.5:9910 -cvauth=token,/tmp/token -cvvrf=default -disableaaa -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| IDF3_AGG | Vlan4094 | 192.168.1.11 | Port-Channel47 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id IDF3_AGG
   local-interface Vlan4094
   peer-address 192.168.1.11
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

### Internal VLAN Allocation Policy Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 310 | IDF3-Data | - |
| 320 | IDF3-Voice | - |
| 330 | IDF3-Guest | - |
| 4094 | MLAG_PEER | MLAG |

### VLANs Device Configuration

```eos
!
vlan 310
   name IDF3-Data
!
vlan 320
   name IDF3-Voice
!
vlan 330
   name IDF3-Guest
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
| Ethernet49 | SPINE-1_Ethernet5 | *trunk | *310,320,330 | *- | *- | 49 |
| Ethernet50 | SPINE-2_Ethernet5 | *trunk | *310,320,330 | *- | *- | 49 |
| Ethernet51 | MEMBER-LEAF-3C_Ethernet49 | *trunk | *310,320,330 | *- | *- | 51 |
| Ethernet52 | MEMBER-LEAF-3D_Ethernet49 | *trunk | *310,320,330 | *- | *- | 52 |
| Ethernet53/1 | MEMBER-LEAF-3E_Ethernet49 | *trunk | *310,320,330 | *- | *- | 531 |

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
interface Ethernet51
   description MEMBER-LEAF-3C_Ethernet49
   no shutdown
   channel-group 51 mode active
!
interface Ethernet52
   description MEMBER-LEAF-3D_Ethernet49
   no shutdown
   channel-group 52 mode active
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
| Port-Channel49 | SPINES_Po5 | switched | trunk | 310,320,330 | - | - | - | - | 49 | - |
| Port-Channel51 | MEMBER-LEAF-3C_Po49 | switched | trunk | 310,320,330 | - | - | - | - | 51 | - |
| Port-Channel52 | MEMBER-LEAF-3D_Po49 | switched | trunk | 310,320,330 | - | - | - | - | 52 | - |
| Port-Channel531 | MEMBER-LEAF-3E_Po49 | switched | trunk | 310,320,330 | - | - | - | - | 531 | - |

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
   description SPINES_Po5
   no shutdown
   switchport
   switchport trunk allowed vlan 310,320,330
   switchport mode trunk
   mlag 49
!
interface Port-Channel51
   description MEMBER-LEAF-3C_Po49
   no shutdown
   switchport
   switchport trunk allowed vlan 310,320,330
   switchport mode trunk
   mlag 51
!
interface Port-Channel52
   description MEMBER-LEAF-3D_Po49
   no shutdown
   switchport
   switchport trunk allowed vlan 310,320,330
   switchport mode trunk
   mlag 52
!
interface Port-Channel531
   description MEMBER-LEAF-3E_Po49
   no shutdown
   switchport
   switchport trunk allowed vlan 310,320,330
   switchport mode trunk
   mlag 531
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan4094 | MLAG_PEER | default | 1500 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan4094 |  default  |  192.168.1.10/31  |  -  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan4094
   description MLAG_PEER
   no shutdown
   mtu 1500
   no autostate
   ip address 192.168.1.10/31
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

## System L1

### Unsupported interface configurations

| Unsupported Configuration | action |
| ---------------- | -------|
| Speed | error |
| Error correction | error |

### System L1 Configuration

```eos
!
system l1
   unsupported speed action error
   unsupported error-correction action error
```

## EOS CLI

```eos
!
username admin privilege 15 role network-admin secret 5 $1$5O85YVVn$HrXcfOivJEnISTMb6xrJc.
```
