# N4 — Traffic Analysis Lab

**Domain:** Networking
**Tools:** Wireshark, curl
**Platform:** Kali Linux
**Author:** Anam Batool

---

## Objective
Capture and analyze real network traffic to understand
how protocols work and how to detect suspicious activity.

---

## Lab Environment
| Component | Details |
|-----------|---------|
| Attacker machine | Kali Linux |
| Target | example.com (external) |
| Interface | eth0 |

---

## Phase 1 — HTTP Traffic Analysis
- Captured full HTTP conversation with curl
- Observed DNS query → TCP handshake → HTTP GET → 200 OK → FIN
- Identified Host and User-Agent fields in packet headers

## Phase 2 — Suspicious Traffic Detection
- Simulated malware traffic using fake User-Agent
- Used Wireshark filter: http.user_agent contains "BotNet"
- Successfully isolated malicious packet from normal traffic

---

## Key Findings
| Finding | Risk |
|---------|------|
| User-Agent manipulation possible | 🔴 High |
| HTTP traffic fully visible in plaintext | 🔴 High |
| DNS queries expose browsing activity | 🟠 Medium |

---

## Disclaimer
All activities performed in a controlled personal home lab.
For educational and portfolio purposes only.
