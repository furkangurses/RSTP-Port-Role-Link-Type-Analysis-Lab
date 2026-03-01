![Topology Diagram]()

# RSTP Port Role & Link-Type Analysis Lab

---

## 🎯 Lab Objective

This lab analyzes **Rapid Spanning Tree Protocol (RSTP – IEEE 802.1w)** behavior in a multi-switch topology.

Objectives:

- Identify the Root Bridge
- Determine port roles without relying on CLI
- Understand root cost calculation
- Analyze designated and alternate port selection
- Configure link types (point-to-point / shared)
- Implement PortFast (edge ports)

---

## 🌐 Topology Overview

- 4 Switches (SW1, SW2, SW3, SW4)
- Single VLAN (VLAN 1)
- Default bridge priority: 32769 (32768 + VLAN 1)
- Mix of:
  - Direct switch-to-switch links
  - Hub-based shared segment
  - End-device connections

### Root Bridge Selection

Since all switches share the same priority:

- The lowest MAC address determines the Root Bridge.
- **SW1** has the lowest MAC address → becomes Root Bridge.

---

## ⚙️ Configuration Steps

### 1️⃣ Root Bridge Identification
- Compared Bridge IDs (priority + MAC)
- Determined SW1 as Root Bridge

### 2️⃣ Port Role Determination (Without CLI)
- Identified Root Ports based on lowest path cost
- Determined Designated Ports per collision domain
- Identified Alternate (discarding) ports

### 3️⃣ Verified Port Roles
Validated roles using:

```
enable
show spanning-tree
```

### 4️⃣ Configured Link Types
Manually set point-to-point links where appropriate.

### 5️⃣ Enabled PortFast
Configured edge ports for end devices to allow immediate forwarding.

---

## 📝 Commands Used

```
enable
configure terminal

show spanning-tree

interface range f0/1 - 2
spanning-tree link-type point-to-point

interface f0/24
spanning-tree portfast

interface range f0/23 - 24
spanning-tree portfast
```

---

## 🧠 Technical Explanation

This lab demonstrates how RSTP makes forwarding decisions logically rather than randomly.

Key concepts:

- Root Bridge is elected using Bridge ID.
- Each non-root switch selects one Root Port (lowest total cost to root).
- Each collision domain allows only one Designated Port.
- Remaining redundant links become Alternate (discarding).
- Shared segments (hub connections) affect designated port logic.
- Edge ports (PortFast) bypass listening/learning states.

Important insight:

A root bridge does not always have all ports designated — if multiple interfaces connect to the same shared collision domain, only one can be designated.

RSTP improves convergence by pre-calculating alternate paths.

---

## 🌍 Real-World Use Case

This lab reflects scenarios such as:

- Preventing Layer 2 loops in redundant switch environments
- Troubleshooting broadcast storms
- Verifying why a link is in blocking/discarding state
- Ensuring user ports come up instantly with PortFast
- Understanding behavior in mixed full-duplex and shared environments

Applicable in enterprise access layer deployments.

---

## 🛠️ Skills Gained

- RSTP root election analysis
- Port role calculation without CLI dependency
- Root cost evaluation
- Designated vs Alternate port logic
- Link-type impact on convergence
- Edge port configuration
- STP output interpretation

---

## ⚡ Possible Improvements

- Configure manual root bridge priority
- Add multiple VLANs to observe per-VLAN root election
- Introduce link failure simulation to observe convergence
- Compare behavior with legacy STP (802.1D)

---

## 🧩 Troubleshooting Notes

- If an unexpected port is blocking:
  - Verify root cost
  - Compare Bridge IDs
  - Check link type (shared vs point-to-point)

- If end devices experience delay:
  - Confirm PortFast is enabled

- If multiple ports connect to a hub:
  - Only one can be Designated per collision domain

- Always validate with:

```
show spanning-tree
```
