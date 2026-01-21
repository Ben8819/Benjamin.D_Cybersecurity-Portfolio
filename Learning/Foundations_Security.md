# 🔐 Security Foundations  
*A clear, practical overview of the core cybersecurity concepts I rely on as a future SOC Analyst.*

This document summarizes the essential security knowledge I built during my Security+ preparation, labs, and hands-on practice. This list is non exhaustive.

---

# 1. Security Principles

### CIA Triad  
- **Confidentiality** → protect data from unauthorized access  
- **Integrity** → ensure data cannot be tampered with  
- **Availability** → ensure systems remain accessible  

### Zero Trust  
Modern security is no longer perimeter-based.  
Key rules:
- Never trust by default  
- Verify every request  
- Enforce least privilege  
- Continuously monitor context  

---

# 2. Threat Actors & TTPs

### Threat Actor Types
- **Cybercriminals**  
- **Nation-state APTs**  
- **Insiders**  
- **Hacktivists**  
- **Script kiddies**  

### Techniques (MITRE ATT&CK)
- **Initial Access** → phishing, scanning, exploitation  
- **Execution** → PowerShell, scripts  
- **Persistence** → services, registry, scheduled tasks  
- **Privilege Escalation**  
- **Defense Evasion**  
- **C2**  
- **Exfiltration**  

Understanding these helps align alerts to attacker behaviour.

---

# 3. Network Security Foundations

### Core Concepts
- Segmentation  
- VLANs  
- ACLs  
- Ports & protocols  
- VPN & tunneling  
- IDS vs IPS  
- Firewall design  

### Why it matters  
SOC work depends heavily on understanding traffic baselines, normal behaviour, and anomalies.

---

# 4. Identity & Access Management (IAM)

### Key Elements
- MFA  
- SSO  
- OAuth / OIDC  
- Kerberos  
- Certificates  
- Account lifecycle  

### Principles
- Least privilege  
- Separation of duties  
- Just-in-time access  
- Password hygiene  

Identity abuses (lateral movement, credential theft, brute force) are among the most common SOC alerts.

---

# 5. Endpoint Security

### Windows  
- Event Viewer  
- Sysmon  
- Registry monitoring  
- Process behaviour  

### Linux  
- Syslog  
- auth.log  
- Permissions  
- Services  

### EDR Concepts  
- Real-time telemetry  
- Process trees  
- Behaviour-based detection  
- Automatic isolation  

---

# 6. Cryptography Basics

### Key Concepts
- Hashing (SHA-256)  
- Encryption (AES, RSA)  
- TLS handshake  
- Certificates & PKI  
- Digital signatures  

### Why analysts need this  
Understanding encrypted traffic, certificate anomalies, and hash validation improves triage quality.

---

# 7. Risk, Governance & Controls

### Frameworks  
- NIST CSF  
- ISO 27001  
- SOC processes  
- Change management  

### Controls  
- Administrative  
- Technical  
- Physical  

This helps understand why certain logs or events matter and how organizations design detection pipelines.

---

# Summary

These fundamentals support:
- Clear alert triage  
- Accurate incident classification  
- Better communication with IR teams  
- Context-aware investigation  
- Faster pattern recognition  

This is the foundation everything else in my portfolio builds upon.
