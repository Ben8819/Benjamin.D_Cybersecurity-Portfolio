# 🖥 Operating Systems for Cybersecurity  
*A practical overview of the OS knowledge I use during SOC analysis.*

---

# 1. Windows Internals (Relevant for SOC)

### Key Components
- Registry  
- Services  
- Processes & threads  
- Event Viewer  
- Sysmon  

### Important Event IDs
- **4624** → successful login  
- **4625** → failed login  
- **4688** → process creation  
- **7045** → new service installed  
- **4104** → PowerShell script block logging  
- **1 (Sysmon)** → process creation  
- **3 (Sysmon)** → network connections  

### Why this matters  
Most malware targets Windows endpoints.

---

# 2. Linux Internals

### Important Logs  
- `/var/log/auth.log`  
- `/var/log/syslog`  
- `journalctl`  

### Concepts  
- Permissions (`rwx`)  
- Processes (`ps`, `top`)  
- Services (`systemctl`)  
- File system hierarchy  

### Security Relevance  
- SSH brute force  
- Unauthorized sudo  
- Reverse shells  
- Suspicious systemd units  

---

# 3. Common Attack Vectors

- PowerShell misuse  
- Service persistence  
- Cron job persistence  
- Credential dumping  
- Lateral movement  
- “Living off the land” techniques  

---

# Summary

Understanding Windows and Linux internals is crucial for:
- Interpreting logs  
- Identifying anomalies  
- Detecting persistence  
- Supporting incident response  
