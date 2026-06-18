# N7 — Intrusion Detection System with Snort

**Domain:** Networking / Cybersecurity
**Tools:** Snort 3, Nmap, ping
**Platform:** Kali Linux
**Author:** Anam Batool

---

## Objective
Build a live Intrusion Detection System using Snort 3,
write custom detection rules, and trigger real alerts
from ICMP and Nmap SYN scan traffic.

---

## Lab Environment
| Component | Details |
|-----------|---------|
| IDS Engine | Snort 3.12.2.0 |
| Interface | eth0 |
| Rules file | /etc/snort/rules/local.rules |
| Log output | outputs/snort_alerts.txt |

---

## Custom Rules Written
| Rule | SID | Detects |
|------|-----|---------|
| ICMP Ping Detected | 1000001 | Any ping traffic |
| Nmap SYN Scan Detected | 1000002 | TCP SYN port scans |

---

## Phase 1 — ICMP Detection
- Wrote custom rule to detect ICMP type 8 (ping requests)
- Triggered alerts by pinging router
- Confirmed bidirectional detection (request + reply)

## Phase 2 — Nmap Scan Detection
- Wrote custom rule to detect TCP SYN scans
- Ran nmap -sS against router
- Snort triggered alerts in real time

---

## Key Findings
| Finding | Detail |
|---------|--------|
| Live IDS working | Snort 3 detecting traffic in real time |
| Custom rules functional | Both ICMP and SYN scan rules firing |
| Nmap detected | Port scans visible to defender immediately |

---

## Defenses
- Deploy IDS on network perimeter and internal segments
- Write rules for known attacker tools (nmap, metasploit)
- Forward alerts to SIEM for correlation

---

## Disclaimer
All activities performed in a controlled personal home lab.
For educational and portfolio purposes only.
