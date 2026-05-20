# ccna-network-access-lab
CCNA 200-301 Network Access lab: VLANs, EtherChannel (LACP), trunk hardening, STP, and CDP across two switches.
```markdown

# Multi-Switch Network with VLANs

## Project Overview

This project focuses on building a small Layer 2 switched network using Cisco Packet Tracer. The lab includes VLAN configuration, EtherChannel implementation using LACP, trunk hardening, VLAN isolation testing, and basic switch security features.

The purpose of this project is to strengthen my understanding of the CCNA Network Access module topics 2.1 to 2.5, which contribute significantly to the CCNA exam objectives. The lab simulates how a real small-office LAN environment operates using multiple switches.

---

# Goals and Scope

The main goals of this project were:

* Build a multi-switch Layer 2 network
* Configure VLAN segmentation
* Configure and verify EtherChannel using LACP
* Implement trunk hardening with a dedicated native VLAN
* Test VLAN isolation and same-VLAN communication
* Enable switch protection and optimization features such as:

  * CDP
  * PortFast
  * BPDU Guard

This project helped me gain practical experience with:

* VLANs
* Trunking
* EtherChannels
* Switchport configuration
* Network segmentation
* Basic switch security
* Connectivity verification and troubleshooting

---

# Lab Environment

## Tools Used

* Cisco Packet Tracer
* GitHub
* Cisco IOS CLI

---

# Repository Setup

In this step, I created a public GitHub repository called:

`Multi-Switch Network with VLANs`

The purpose of the repository is to:

* Store my Packet Tracer project file
* Upload screenshots of the topology and configurations
* Document commands used during the lab
* Keep troubleshooting notes and explanations
* Track my networking learning journey

---

# Saving the Topology File

The Packet Tracer file was saved locally as:

`ccna network access lab`

The file was stored in my Downloads folder before being uploaded to GitHub.

---

# Building the Two-Switch Topology

## Topology Overview

The network consists of:

* Two Cisco switches
* Multiple PCs connected to access ports
* Inter-switch links bundled into an EtherChannel

The topology was designed to simulate communication between devices connected across multiple switches inside the same LAN.

---

# Inter-Switch EtherChannel Port Assignments

The switches were connected using multiple interfaces bundled together into a single logical EtherChannel link.

EtherChannel was implemented to:

* Increase bandwidth
* Provide redundancy
* Prevent Layer 2 loops
* Improve overall link efficiency

LACP (Link Aggregation Control Protocol) was used to dynamically negotiate the EtherChannel between both switches.

---

# Configuring VLANs Across Both Switches

## VLAN Configuration

Multiple VLANs were created across both switches to logically separate devices into different broadcast domains.

Example VLAN purposes:

| VLAN    | Purpose       |
| ------- | ------------- |
| VLAN 10 | User Network  |
| VLAN 20 | Staff Network |
| VLAN 99 | Native VLAN   |

---

# Native VLAN 99 and Trunk Hardening Rationale

VLAN 99 was configured as the native VLAN for the trunk links.

A native VLAN is the VLAN that carries untagged traffic across a trunk port in a Layer 2 network.

When traffic arrives without a VLAN tag, the switch automatically places that traffic into the native VLAN.

## Why Trunk Hardening Was Important

Trunk hardening was implemented to improve security and reduce unnecessary traffic.

Benefits include:

* Preventing VLAN hopping attacks
* Limiting which VLANs are allowed across the trunk
* Separating management/native traffic from user VLANs
* Improving overall network organization and security


# Deploying the LACP EtherChannel Trunk

## EtherChannel and Trunk Configuration

In this step, I configured:

* EtherChannel using LACP
* Trunk ports between the switches
* Allowed VLANs across the trunk
* Native VLAN settings

The EtherChannel grouped multiple physical interfaces into one logical interface called a Port-Channel.

This configuration allows:

* Better bandwidth utilization
* Redundancy if one cable fails
* More stable inter-switch communication


# Verifying Cross-Switch VLAN Connectivity

## IP Addressing and Connectivity Testing

In this step, I assigned IP addresses to PCs and tested connectivity.

The results confirmed:

* Devices in the same VLAN could successfully communicate across switches
* Devices in different VLANs could not communicate

This behavior is expected because VLANs create separate Layer 2 broadcast domains.

Without a Layer 3 device such as:

* A router
* A Layer 3 switch
* Router-on-a-stick configuration

inter-VLAN communication cannot occur.


# Confirming Inter-VLAN Isolation

I tested communication by pinging devices across the network.

## Results

### Successful Tests

* PCs in the same VLAN across different switches successfully pinged each other.

### Failed Tests

* PCs in different VLANs failed to ping each other.

This confirmed that:

* VLAN segmentation was functioning correctly
* Broadcast domains were isolated successfully
* Inter-VLAN routing was not present


# Enabling CDP, PortFast, and BPDU Guard

Additional switch features were configured to improve management and security.

## CDP (Cisco Discovery Protocol)

CDP was enabled to allow Cisco devices to discover directly connected Cisco neighbors.

This helps with:

* Network discovery
* Troubleshooting
* Device identification


## PortFast

PortFast was enabled on access ports connected to end devices.

Benefits of PortFast:

* Devices connect faster to the network
* Bypasses lengthy STP listening and learning states
* Improves startup connectivity for PCs

## BPDU Guard

BPDU Guard was enabled on PortFast-enabled ports.

This feature protects the network from accidental switch connections that could create Layer 2 loops.

If a BPDU is received on a PortFast port:

* The port automatically shuts down
* Potential spanning-tree issues are prevented


# Skills Learned

Through this project, I gained hands-on experience with:

* VLAN creation and management
* Access and trunk port configuration
* EtherChannel configuration using LACP
* Native VLAN configuration
* Trunk hardening techniques
* VLAN isolation testing
* Basic switch security practices
* Troubleshooting Layer 2 connectivity
* Cisco CLI navigation and configuration

# Key Commands Used

## VLAN Commands

```bash
vlan 10
name USERS

vlan 20
name STAFF

vlan 99
name NATIVE
```

## Trunk Configuration

```bash
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 10,20,99
```

## EtherChannel Configuration

```bash
channel-group 1 mode active

interface port-channel 1
switchport mode trunk
```

## PortFast and BPDU Guard

```bash
spanning-tree portfast
spanning-tree bpduguard enable
```

# Troubleshooting Notes

During the project, I encountered and resolved several Layer 2 switching issues while configuring VLANs, trunks, and EtherChannels.

These troubleshooting steps helped me better understand how switches communicate and how VLAN consistency affects trunk links.


## Native VLAN Mismatch Issue

One of the main issues I experienced during the project was a native VLAN mismatch between the switches.

### Problem

The trunk ports between the switches were configured with different native VLANs.

This caused:

* CDP warnings
* Trunk inconsistencies
* Potential VLAN traffic issues
* Security concerns

The switches detected that the native VLAN configured on one side of the trunk did not match the native VLAN configured on the other side.


## Symptoms Observed

I observed warning messages similar to:

```bash
%CDP-4-NATIVE_VLAN_MISMATCH
```

This indicated that the switches were exchanging CDP information and detecting different native VLAN configurations on the connected trunk ports.


## Root Cause

The issue happened because:

* One switch trunk port was configured with VLAN 99 as the native VLAN
* The other switch trunk port was still using the default native VLAN 1

Since native VLANs must match on both sides of a trunk link, the mismatch triggered warnings and created inconsistency across the trunk.


## Troubleshooting Process

To troubleshoot the issue, I:

1. Verified the trunk interfaces
2. Checked allowed VLANs
3. Checked the configured native VLAN on both switches
4. Compared the switchport trunk settings
5. Verified EtherChannel consistency

Useful commands included:

```bash
show interfaces trunk
show running-config
show etherchannel summary
show cdp neighbors detail
```

## Fix Applied

I corrected the mismatch by configuring the same native VLAN on both trunk interfaces.

### Configuration Example

```bash
switchport trunk native vlan 99
```

This command was applied consistently on both sides of the trunk and on the Port-Channel interface.

## Result After the Fix

After correcting the native VLAN mismatch:

* CDP warnings disappeared
* The trunk operated correctly
* VLAN traffic passed successfully
* EtherChannel consistency improved
* Cross-switch VLAN communication worked as expected

This troubleshooting process helped me understand the importance of VLAN consistency in trunk links and reinforced the importance of verification commands in Cisco networking.

## Additional Verification Steps

During the project, I also verified:

* VLAN membership assignments
* Trunk operational status
* EtherChannel operational state
* Allowed VLAN lists
* PC IP addressing
* Port configurations
* Connectivity between same-VLAN devices
* Isolation between different VLANs

Useful verification commands included:

```bash
show vlan brief
show interfaces trunk
show etherchannel summary
show spanning-tree
show cdp neighbors
```
# Conclusion

This project gave me practical exposure to how Layer 2 enterprise switching works in real-world environments.

By configuring VLANs, EtherChannels, and trunk security features, I developed a stronger understanding of:

* Network segmentation
* Redundancy
* Switch communication
* Basic LAN security
* CCNA Network Access concepts

This lab serves as a strong foundation for more advanced networking topics such as:

* Inter-VLAN routing
* Spanning Tree Protocol optimization
* Layer 3 switching
* Enterprise campus design
* Network automation

# Author

Created by Nondumiso Mbuyazi as part of my CCNA networking and infrastructure learning journey.
