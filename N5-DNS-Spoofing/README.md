#  DNS Spoofing Lab

**Domain:** Networking / Cybersecurity
**Tools:** dnsmasq, ettercap, curl, nmap
**Platform:** Kali Linux
**Author:** Anam Batool

---

## Objective
Demonstrate DNS spoofing attack to understand how attackers
redirect victims to malicious servers and how defenders detect it.

---

## Lab Environment
| Component | Details |
|-----------|---------|
| Attacker machine | Kali Linux |
| Target domain | example.com |
| Interface | eth0 |

---

## Phase 1 — ARP Poisoning Attempt
- Used ettercap to attempt ARP poisoning between router and targets
- Router ARP protection prevented full poisoning
- Documented limitation — real finding in itself

## Phase 2 — DNS Spoofing via dnsmasq
- Configured dnsmasq to resolve example.com to Kali IP
- Verified spoof with nslookup — returned 192.168.X.104
- Confirmed redirect with curl — Kali web server responded

## Phase 3 — Impact Demonstration
- Python HTTP server served content from Kali machine
- Any victim using spoofed DNS lands on attacker server
- Real attack scenario: replace with fake login page

---

## Key Findings
| Finding | Risk |
|---------|------|
| DNS spoofing redirects traffic silently | 🔴 Critical |
| Victims see correct URL but wrong server | 🔴 Critical |
| Router ARP protection partially effective | 🟢 Positive |

---

## Defenses Against DNS Spoofing
- Use DNSSEC
- Use encrypted DNS (DoH or DoT)
- Monitor for rogue DHCP/DNS servers on network

---

## Disclaimer
All activities performed in a controlled personal home lab.
For educational and portfolio purposes only.
