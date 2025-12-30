# 🚀 Day 1 Quick-Start Guide - SecureNet Lab

## Step 1: VirtualBox Network Setup (10 minutes)

Open VirtualBox and configure each VM's network adapters:

### OPNsense Firewall (4 Adapters)
```
Settings → Network:

Adapter 1: NAT                    ← Internet access
Adapter 2: Internal Network       ← Name: lan_net
Adapter 3: Internal Network       ← Name: dmz_net  
Adapter 4: Internal Network       ← Name: attack_net
```

### Ubuntu Server (Suricata)
```
Adapter 1: Internal Network       ← Name: lan_net
```

### Metasploitable2
```
Adapter 1: Internal Network       ← Name: dmz_net
```

### Kali Linux
```
Adapter 1: Internal Network       ← Name: attack_net
```

---

## Step 2: OPNsense Configuration (20 minutes)

### 2.1 Boot OPNsense & Login
```
Username: root
Password: opnsense
```

### 2.2 Assign Interfaces (Option 1)
```
WAN  → em0 (or vtnet0)
LAN  → em1
OPT1 → em2 (rename to DMZ later)
OPT2 → em3 (rename to ATTACK later)
```

### 2.3 Set Interface IPs (Option 2)
```
LAN:    10.10.10.1/24   DHCP: 10.10.10.100-200
OPT1:   10.10.20.1/24   DHCP: 10.10.20.100-200
OPT2:   10.10.30.1/24   DHCP: 10.10.30.100-200
WAN:    DHCP (automatic)
```

### 2.4 Access Web GUI
From Ubuntu (after getting IP):
```
https://10.10.10.1
Username: root
Password: opnsense
```

### 2.5 Create Firewall Rules (GUI)
Go to: Firewall → Rules → [Each Interface]

**LAN Rules:**
- Allow LAN to Any

**DMZ Rules:**
- Block DMZ to LAN
- Block DMZ to ATTACK
- Allow DMZ to WAN (limited)

**ATTACK Rules:**
- Allow ATTACK to DMZ only
- Block ATTACK to LAN
- Block ATTACK to WAN

---

## Step 3: Suricata on Ubuntu (30 minutes)

### 3.1 Get IP Address
```bash
ip a
# Note your IP (should be 10.10.10.x)
```

### 3.2 Install Suricata
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install suricata suricata-update -y
```

### 3.3 Update Rules
```bash
sudo suricata-update
```

### 3.4 Find Interface Name
```bash
ip a
# Usually: enp0s3 or eth0
```

### 3.5 Edit Configuration
```bash
sudo nano /etc/suricata/suricata.yaml
```

Change these settings:
```yaml
vars:
  address-groups:
    HOME_NET: "[10.10.10.0/24, 10.10.20.0/24, 10.10.30.0/24]"

af-packet:
  - interface: enp0s3   # Your interface name
```

### 3.6 Add Custom Rules
```bash
# Copy the custom.rules file content to:
sudo nano /etc/suricata/rules/custom.rules

# Add to suricata.yaml under rule-files:
sudo nano /etc/suricata/suricata.yaml
# Add this line:
#   - /etc/suricata/rules/custom.rules
```

### 3.7 Test & Start
```bash
# Validate config
sudo suricata -T -c /etc/suricata/suricata.yaml

# Start Suricata
sudo systemctl enable suricata
sudo systemctl start suricata

# Check status
sudo systemctl status suricata
```

---

## Step 4: Verify Everything Works

### Test 1: Ping Between VMs
From Kali (10.10.30.x):
```bash
ping 10.10.20.10  # Should work (ATTACK → DMZ allowed)
ping 10.10.10.10  # Should FAIL (ATTACK → LAN blocked)
```

### Test 2: Check Suricata Logs
From Ubuntu:
```bash
sudo tail -f /var/log/suricata/fast.log
```

Run nmap from Kali → Should see alerts appear!

---

## 📸 Screenshots to Take

1. [ ] VirtualBox network settings for each VM
2. [ ] OPNsense interface assignments
3. [ ] OPNsense firewall rules
4. [ ] Suricata running status
5. [ ] Suricata test alert

---

## ⚠️ Troubleshooting

**No network on VMs?**
- Check VirtualBox adapter settings
- Verify internal network names match exactly

**Can't access OPNsense web GUI?**
- Ensure LAN interface has IP (option 2 in console)
- Try: `https://10.10.10.1` (not http)

**Suricata won't start?**
- Check config syntax: `sudo suricata -T -c /etc/suricata/suricata.yaml`
- Check interface name matches

---

**Save this file and use it as your checklist!**
