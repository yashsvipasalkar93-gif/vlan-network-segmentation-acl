# 🔒 VLAN Network Segmentation with ACL-Based Access Control

> Isolating a simulated attacker from sensitive internal resources using VLAN segmentation and router-based ACLs — built and tested in Cisco Packet Tracer.

## 📌 Objective

Flat, unsegmented networks let any compromised device freely reach every other device — including sensitive HR and IT systems. This project demonstrates how **VLAN segmentation combined with Access Control Lists (ACLs)** can contain a compromised or malicious host, preventing lateral movement to critical resources while preserving normal business connectivity.

## 🖥️ Topology Overview

| Device | Role | VLAN |
|--------|------|------|
| PC0 | HR | VLAN 10 (TRUSTED) |
| PC1 | IT Team | VLAN 10 (TRUSTED) |
| PC2 | Guests | VLAN 20 (GUESTS) |
| PC3 | Simulated Attacker | VLAN 30 (ATTACKER_ZONE) |
| Switch0 | Layer 2 switch, VLAN-aware trunking | — |
| Router 2911 | Inter-VLAN routing via Router-on-a-Stick | — |

**Before Segmentation:** All 4 devices sit on a single flat network — any device can reach any other device, including HR and IT systems.

**After Segmentation:** Devices are separated into 3 VLANs, with inter-VLAN routing handled by a Router-on-a-Stick configuration and traffic explicitly controlled via ACL.

## ⚙️ Configuration Summary

**VLAN Setup**
- VLAN 10 — TRUSTED (HR + IT)
- VLAN 20 — GUESTS
- VLAN 30 — ATTACKER_ZONE

**Router-on-a-Stick (Inter-VLAN Routing)**

Router 2911 configured with sub-interfaces to route between VLANs over a single trunk link:
