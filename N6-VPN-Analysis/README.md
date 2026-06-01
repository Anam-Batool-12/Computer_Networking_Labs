# — VPN Setup & Analysis

**Domain:** Networking / Cybersecurity
**Tools:** WireGuard, Wireshark, ping
**Platform:** Kali Linux
**Author:** Anam Batool

---

## Objective
Set up a WireGuard VPN server and client, capture tunnel
traffic in Wireshark, and compare encrypted vs unencrypted traffic.

---

## Lab Environment
| Component | Details |
|-----------|---------|
| VPN Server | Kali Linux — 10.0.0.1 |
| VPN Client | Kali Linux — 10.0.0.2 |
| Interface | wg0 |
| Protocol | WireGuard (UDP 51820) |

---

## Phase 1 — VPN Server Setup
- Generated server and client key pairs
- Configured wg0 interface at 10.0.0.1/24
- Added client as peer with allowed IPs

## Phase 2 — Tunnel Verification
- Brought up client interface at 10.0.0.2
- Captured ICMP traffic on wg0 in Wireshark
- Confirmed packets traveling through encrypted tunnel

## Phase 3 — Traffic Comparison
- Compared wg0 (encrypted) vs eth0 (plaintext)
- eth0: full visibility of source, destination, protocol
- wg0: only encrypted UDP visible, contents hidden

---

## Key Findings
| Finding | Detail |
|---------|--------|
| VPN tunnel established | WireGuard wg0 interface |
| Traffic encrypted | No payload visible in Wireshark |
| Regular traffic exposed | Full details visible on eth0 |

---

## Defenses
- Always use VPN on public/untrusted networks
- WireGuard preferred over OpenVPN — faster, modern crypto
- Combine with DNS over HTTPS to prevent DNS leaks

---

## Disclaimer
All activities performed in a controlled personal home lab.
For educational and portfolio purposes only.
