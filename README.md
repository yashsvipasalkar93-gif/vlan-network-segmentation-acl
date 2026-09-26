\# 🔒 VLAN Network Segmentation with ACL-Based Access Control



> Isolating a simulated attacker from sensitive internal resources using VLAN segmentation and router-based ACLs — built and tested in Cisco Packet Tracer.



\## 📌 Objective



Flat, unsegmented networks let any compromised device freely reach every other device — including sensitive HR and IT systems. This project demonstrates how \*\*VLAN segmentation combined with Access Control Lists (ACLs)\*\* can contain a compromised or malicious host, preventing lateral movement to critical resources while preserving normal business connectivity.



\## 🖥️ Topology Overview



| Device | Role | VLAN |

|--------|------|------|

| PC0 | HR | VLAN 10 (TRUSTED) |

| PC1 | IT Team | VLAN 10 (TRUSTED) |

| PC2 | Guests | VLAN 20 (GUESTS) |

| PC3 | Simulated Attacker | VLAN 30 (ATTACKER\_ZONE) |

| Switch0 | Layer 2 switch, VLAN-aware trunking | — |

| Router 2911 | Inter-VLAN routing via Router-on-a-Stick | — |



\*\*Before Segmentation:\*\* All 4 devices sit on a single flat network — any device can reach any other device, including HR and IT systems.



\*\*After Segmentation:\*\* Devices are separated into 3 VLANs, with inter-VLAN routing handled by a Router-on-a-Stick configuration and traffic explicitly controlled via ACL.



\## ⚙️ Configuration Summary



\*\*VLAN Setup\*\*

\- VLAN 10 — TRUSTED (HR + IT)

\- VLAN 20 — GUESTS

\- VLAN 30 — ATTACKER\_ZONE



\*\*Router-on-a-Stick (Inter-VLAN Routing)\*\*



Router 2911 configured with sub-interfaces to route between VLANs over a single trunk link:

```

Gi0/0.10 — VLAN 10 gateway

Gi0/0.20 — VLAN 20 gateway

Gi0/0.30 — VLAN 30 gateway

```



\*\*Extended ACL 100 — Access Control Logic\*\*



Applied on the router to explicitly block the attacker zone from reaching trusted resources, while leaving all other traffic unaffected:

```

deny ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255

permit ip any any

```

This denies any traffic originating from VLAN 30 (Attacker Zone) destined for VLAN 10 (Trusted/HR+IT), while permitting everything else — including Guest traffic, which remains unaffected by this rule.



\## ✅ Test Results



| Test | Result | Outcome |

|------|--------|---------|

| Attacker (VLAN 30) → HR/IT (VLAN 10) | 100% packet loss | ✅ Blocked as intended |

| Attacker (VLAN 30) → Guests (VLAN 20) | 0% packet loss | ✅ Unaffected, as expected |



This confirms the ACL successfully isolates the attacker zone from trusted resources \*\*without breaking legitimate guest network functionality\*\* — demonstrating targeted segmentation rather than a blanket network lockdown.



\## 📸 Screenshots



\*\*Before Segmentation — Flat Network\*\*



!\[Before Segmentation](screenshots/topology-before.png)



\*\*After Segmentation — VLAN + ACL Topology\*\*



!\[After Segmentation](screenshots/topology-after.png)



\*\*ACL Configuration\*\*



!\[ACL Config](screenshots/acl-config.png)



\*\*Ping Test: Attacker → HR/IT (Blocked)\*\*



!\[Ping Attacker to HR](screenshots/ping-test-attacker-to-hr.png)



\*\*Ping Test: Attacker → Guests (Successful)\*\*



!\[Ping Attacker to Guests](screenshots/ping-test-attacker-to-guests.png)



\## 🧠 Key Learnings



\- How VLANs alone don't provide security — \*\*routing + ACLs are what actually enforce access control\*\* between segments

\- Practical implementation of Router-on-a-Stick for scalable inter-VLAN routing without needing a dedicated interface per VLAN

\- How to write and apply extended ACLs with precise source/destination targeting rather than broad allow/deny rules

\- The importance of \*\*testing both the "should be blocked" and "should still work" cases\*\* — a segmentation project isn't complete until you verify it doesn't break legitimate traffic too



\## 🛠️ Tools Used



\- Cisco Packet Tracer

\- Extended Access Control Lists (ACL 100)

\- Router-on-a-Stick (802.1Q trunking)



\## 📂 Files in This Repository



| File | Description |

|------|--------------|

| `Before\_Segmentation.pkt` | Original flat network topology (no VLANs) |

| `After\_Segmentation.pkt` | Fully segmented topology with VLANs, router, and ACL applied |

| `/screenshots` | Visual documentation of topology and test results |

