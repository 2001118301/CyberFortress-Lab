# LinkedIn Post & Resume Content

## LinkedIn Post

Here's a simple post you can customize:

---

Built a cybersecurity home lab to practice network security skills.

Set up OPNsense as a firewall with three network zones - a trusted LAN, a DMZ for servers, and an attack simulation zone. Configured firewall rules so the attack zone can reach the DMZ but not the internal network.

Deployed Suricata IDS to monitor traffic and wrote custom detection rules. Then ran attacks from Kali Linux using Metasploit against an intentionally vulnerable target.

The IDS caught everything - port scans, FTP login attempts, and the backdoor exploit. Pretty cool seeing the detection rules I wrote actually trigger on real attack traffic.

Good practice for understanding how network segmentation and intrusion detection work together.

#Cybersecurity #NetworkSecurity #HomeLab

---

## Resume Bullet Points

Pick the ones that fit your resume best:

**Network Security**
- Designed segmented virtual network with isolated security zones using OPNsense firewall
- Configured zone-based firewall policies to prevent lateral movement between network segments

**Intrusion Detection**
- Deployed Suricata IDS and wrote custom detection rules for targeted attack patterns
- Monitored and analyzed security alerts during simulated attack scenarios

**Penetration Testing**
- Conducted network reconnaissance and identified vulnerable services using Nmap
- Exploited vsftpd backdoor vulnerability to demonstrate attack impact

**Tools**
OPNsense, Suricata, Kali Linux, Metasploit, Nmap, VirtualBox

---

## Interview Notes

If someone asks about this project:

"I built a home lab to practice network security. I set up three network zones with a firewall in between - basically simulating how a company would separate their internal network from less trusted areas. Then I put an IDS on the network and wrote some detection rules. After that I attacked the vulnerable server from a different zone and watched the IDS catch everything. It was useful for understanding how segmentation and detection work in practice."
