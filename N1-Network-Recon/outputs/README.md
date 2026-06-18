# N1 — Network Recon & Asset Discovery Lab

**Domain:** Networking  
**Tools:** nmap, netdiscover, whois  
**Platform:** Kali Linux  
**Author:** Anam Batool 

---

## Objective

Perform network reconnaissance to identify live hosts, enumerate open services,
detect vulnerabilities, and gather domain intelligence using industry-standard tools.

---

## Lab Environment

| Component | Details |
|-----------|---------|
| Attacker machine | Kali Linux — kali |
| Target network | 192.168.x.0/24 (home LAN) |
| Interface | eth0 |
| Total hosts found | 5 |

---

## Tools Used

| Tool | Purpose |
|------|---------|
| `netdiscover` | ARP-based host discovery |
| `nmap -sn` | ICMP ping sweep |
| `nmap -sV -sC -Pn` | Service & version detection |
| `nmap --script vuln` | NSE vulnerability scripts |
| `whois` | Domain OSINT & registration lookup |

---

## Phase 1 — Host Discovery

### netdiscover
```bash
sudo netdiscover -r 192.168.x.0/24 -P
```

**Result — 4 hosts found via ARP:**

| IP | MAC Vendor | Device Type |
|----|-----------|-------------|
| 192.168.x.x | ***** | Router/Gateway |
| 192.168.x.x | Unknown (randomized MAC) | Mobile device |
| 192.168.x.x | ***** | Phone/IoT device |
| 192.168.x.x | Unknown (randomized MAC) | Mobile device |

### nmap ping sweep
```bash
sudo nmap -sn 192.168.x.0/24
```

**Result — 5 hosts up** (includes Kali machine itself)

---

## Phase 2 — Service Enumeration (Gateway)

```bash
sudo nmap -sV -sC -Pn 192.168.x.1
```

| Port | State | Service | Version |
|------|-------|---------|---------|
| 21/tcp | filtered | FTP | — |
| 22/tcp | filtered | SSH | — |
| 23/tcp | **open** | **Telnet** | Home Gateway telnetd |
| 53/tcp | open | DNS | — |
| 80/tcp | open | HTTP/SSL | Expired cert (2014–2024) |

---

## Phase 3 — Vulnerability Scan

```bash
sudo nmap --script vuln -Pn 192.168.x.1
```

### Findings

| CVE | Name | Severity | Status |
|-----|------|----------|--------|
| CVE-2014-3566 | SSL POODLE | 🔴 HIGH | VULNERABLE |
| CVE-2010-2333 | LiteSpeed Source Disclosure | 🟠 Medium | Detected |
| CVE-2005-3299 | phpMyAdmin LFI | 🟡 Low | UNKNOWN (likely false positive) |

### CVE-2014-3566 — SSL POODLE (Critical Finding)
SSL 3.0 protocol vulnerability. Allows a man-in-the-middle attacker
on the same network to decrypt encrypted traffic via padding-oracle attack.
Router is still running unpatched SSL 3.0 as of scan date.

### Additional misconfigurations
- **Port 23 Telnet open** — transmits credentials in plaintext, no encryption
- **SSL certificate expired** — issued 2014, expired December 2024

---

## Phase 4 — Domain OSINT (whois)

```bash
whois google.com
```

| Field | Value |
|-------|-------|
| Registrar | MarkMonitor Inc. |
| Created | 1997-09-15 |
| Expires | 2028-09-13 |
| Name Servers | ns1–ns4.google.com |
| DNSSEC | Unsigned |
| Domain locks | 6 (fully protected) |

**Observation:** Enterprise domains use registrar-level locks
(clientDeleteProhibited, serverTransferProhibited) to prevent
unauthorized transfers or hijacking.

---

## Disclaimer

All activities performed in a controlled personal home lab environment
on devices I own. No unauthorized scanning was conducted.
This project is for educational and portfolio purposes only.
