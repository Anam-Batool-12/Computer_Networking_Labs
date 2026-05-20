# N2 — Firewall Rules & Traffic Filtering Lab

**Domain:** Networking  
**Tools:** ufw, iptables  
**Platform:** Kali Linux  
---

## Objective

Configure and test firewall rules using UFW and iptables to control
incoming and outgoing traffic, block insecure protocols, and verify
rule enforcement using nmap.

---

## Lab Environment

| Component | Details |
|-----------|---------|
| Machine | Kali Linux — kalimachine |
| Tools | ufw, iptables, nmap |
| Interface | eth0 |

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `ufw` | User-friendly firewall management |
| `iptables` | Low-level Linux packet filtering |
| `nmap` | Rule verification via port scanning |

---

## Phase 1 — UFW Configuration

### Default policy set
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```
All incoming traffic blocked by default — allowlist approach.

### Rules applied
```bash
sudo ufw allow 22/tcp     # SSH
sudo ufw allow 80/tcp     # HTTP
sudo ufw deny 23/tcp      # Telnet blocked
sudo ufw allow from 192.168.x.0/24
```

### UFW status output



---

## Phase 2 — Rule Verification (nmap)

```bash
sudo nmap -p 22,23,80 localhost
```

| Port | Expected | Result |
|------|---------|--------|
| 22/tcp | Allow | closed (no service, rule active) ✅ |
| 23/tcp | Block | closed (DROP rule active) ✅ |
| 80/tcp | Allow | closed (no service, rule active) ✅ |

---

## Phase 3 — Manual iptables Rules

UFW disabled — rules written directly in iptables:

```bash
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 23 -j DROP
```

### Result

| Chain | Policy | Meaning |
|-------|--------|---------|
| INPUT | DROP | All incoming blocked by default |
| FORWARD | DROP | No packet forwarding |
| OUTPUT | ACCEPT | All outgoing allowed |
UFW is a frontend for iptables — both control the same kernel firewall
- Default deny incoming is best practice — allowlist only what is needed
- Port 23 (Telnet) was found open on router in N1 — this lab demonstrates
  how to properly block it at firewall level
- iptables rules are stateless by default — conntrack module adds
  stateful inspection
- DROP silently discards packets — REJECT sends back an error response
----
## Disclaimer

All activities performed in a controlled personal home lab environment
on devices I own. No unauthorized access was conducted.
This project is for educational and portfolio purposes only.
