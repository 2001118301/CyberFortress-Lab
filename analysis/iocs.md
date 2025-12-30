# Indicators of Compromise (IoCs) - SecureNet Lab

## Attack Summary

| Timestamp | Source IP | Target IP | Attack Type | Severity |
|-----------|-----------|-----------|-------------|----------|
| [TBD] | 10.10.30.10 | 10.10.20.10 | Nmap SYN Scan | Medium |
| [TBD] | 10.10.30.10 | 10.10.20.10 | Service Enumeration | Medium |
| [TBD] | 10.10.30.10 | 10.10.20.10 | vsftpd Exploitation | Critical |
| [TBD] | 10.10.30.10 | 10.10.20.10 | SMB Enumeration | Medium |

## Network-Based Indicators

### Source IPs (Attackers)
- `10.10.30.10` - Kali Linux attack machine

### Target IPs (Victims)
- `10.10.20.10` - Metasploitable2 (vulnerable server)

### Suspicious Ports
| Port | Protocol | Indication |
|------|----------|------------|
| 21 | TCP | FTP login attempts |
| 23 | TCP | Telnet access |
| 445 | TCP | SMB enumeration |
| 6200 | TCP | vsftpd backdoor shell |
| 6667 | TCP | IRC (potential C2) |

## File-Based Indicators
*To be populated after attack simulation*

- Suspicious processes
- Modified files
- Malicious payloads

## Behavioral Indicators

1. **Reconnaissance Phase**
   - High volume of SYN packets (port scanning)
   - ICMP sweep across subnet
   - Service version fingerprinting

2. **Exploitation Phase**
   - vsftpd backdoor trigger (`:)` in username)
   - Connection to port 6200 (backdoor shell)

3. **Post-Exploitation Phase**
   - Command execution
   - Data exfiltration attempts

## Suricata Alert Correlation

| SID | Alert Message | Count |
|-----|--------------|-------|
| 1000001 | Nmap SYN Scan | [TBD] |
| 1000004 | vsftpd Backdoor | [TBD] |
| 1000005 | SMB Enumeration | [TBD] |

---

*Document last updated: [Date]*
