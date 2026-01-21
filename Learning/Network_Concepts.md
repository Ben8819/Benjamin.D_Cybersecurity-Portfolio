# 🌐 Network Concepts for Security Operations

Understanding networks is essential for SOC work.  
This file focuses on the concepts I use to analyze traffic and understand alerts.

---

# 1. OSI vs TCP/IP

### OSI (7 layers)
- Physical  
- Data Link  
- Network  
- Transport  
- Session  
- Presentation  
- Application  

### TCP/IP (4 layers)
- Link  
- Internet  
- Transport  
- Application  

Why I use this:  
Helps identify where an issue (or attack) originates.

---

# 2. Ports & Protocols

### Common:
- **80/443** → HTTP/HTTPS  
- **22** → SSH  
- **3389** → RDP  
- **53** → DNS  
- **445** → SMB  
- **139** → NetBIOS  

### Security relevance  
- Unexpected open ports → recon or compromise  
- Weird DNS patterns → C2 activity  
- Unknown outbound ports → tunneling  

---

# 3. Traffic Baselines

Important for hunting:
- Average DNS queries  
- Normal outbound destinations  
- Expected protocols  
- Usual data volume  

---

# 4. Network Attacks I Can Recognize
- ARP spoofing  
- DNS poisoning  
- Port scanning  
- Brute force  
- Lateral movement  
- Beaconing patterns  

---

# 5. Packet Anatomy

Example TCP packet fields:
- Source IP  
- Destination IP  
- Source port  
- Destination port  
- Flags (SYN, ACK, FIN)  
- Payload  

Understanding these improves packet analysis accuracy.

---

# Summary

Strong network knowledge helps me:
- Diagnose suspicious traffic  
- Identify attack patterns  
- Validate/triage SIEM alerts  
- Understand payloads & protocols  
