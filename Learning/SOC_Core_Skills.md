# 🛡 SOC Core Skills  
*The mindset, workflows, and practical abilities I develop to perform well in a Security Operations Center.*

This document explains how I think, analyze, and operate as a junior SOC analyst.

---

# 1. Alert Triage Workflow (My Method)

### 🔹 Step 1: Validate the Alert  
- Is it genuine or a misfire?  
- Is the event real or log noise?  
- Is the rule logic correct?

### 🔹 Step 2: Gather Context  
- User identity  
- Machine history  
- Known behaviour patterns  
- Recent activity  
- GeoIP vs user location  

### 🔹 Step 3: Correlation  
- Check authentication logs  
- Process execution chains  
- Network connections  
- Outbound anomalies  
- Similar alerts on same host  

### 🔹 Step 4: Classification  
- True positive  
- False positive  
- Benign-but-noteworthy  
- Suspicious → needs escalation  

### 🔹 Step 5: Response  
- Block indicators  
- Isolate host  
- Reset credentials  
- Escalate to IR  
- Document clearly  

---

# 2. Investigation Techniques

### Log analysis (Windows/Linux)  
What I look for:
- Failed logins  
- Unusual process trees  
- Persistence mechanisms  
- Encoded PowerShell commands  
- External connections  
- System changes  

### Network analysis  
What I examine:
- Beaconing patterns  
- High-volume outbound  
- DNS spikes  
- Suspicious ports  
- Long-lived sessions  

### Host forensics  
Using:
- Sysmon  
- Event Viewer  
- Audit logs  
- File system changes  

---

# 3. Threat Hunting Practices

### Baseline → Anomaly → Hypothesis
- Know what “normal” looks like  
- Identify subtle deviations  
- Build a hypothesis  
- Validate it with logs  

### Typical hunts to run:
- Suspicious PowerShell  
- Rare processes  
- Privilege escalation attempts  
- Unusual DNS activity  
- Impossible travel  
- Long-lived TCP connections  

---

# 4. Communication Skills

SOC work isn’t only technical. It requires clarity:
- Precise notes  
- Clear escalation reports  
- Concise explanations  
- Actionable recommendations  
- Proper ticket hygiene  
- Reproducible investigation steps  

This repository itself demonstrates these skills.

---

# Summary

My SOC skillset is built on:
- Analytical thinking  
- Reproducible workflows  
- Curiosity  
- Technical knowledge  
- Strong documentation  
- Continuous learning  

This is the mindset I bring to a SOC team.
