# 🎯 Attack Simulation & Detection Scenarios

! Disclaimer:<br>

All actions described in this section were executed exclusively in a closed, isolated and controlled lab environment owned and managed by me. These activities were performed solely for defensive, educational, and detection-focused purposes and to better understand real-world attack techniques in order to improve security monitoring and incident response. !


## Objective

This section documents the **controlled attack simulations** performed against the lab environment in order to:

- Generate realistic security events
- Validate IDS visibility and SIEM ingestion
- Observe how attacks appear from a SOC analyst perspective
- Confirm end-to-end detection from network traffic to dashboards

---

## How These Scenarios Are Investigated

Each attack scenario documented in this section is analyzed using the structured SOC investigation workflow described in:
[SOC_Investigation_Workflow](../SOC_Investigation_Workflow/SOC_Investigation_Workflow.md)

The purpose is not only to generate alerts, but to demonstrate how those alerts are triaged, correlated, and interpreted from a SOC analyst perspective.

---

## Attack Sources and Targets

### Attack Sources

- **Kali Linux VM**
  - Used as the primary attacker machine
  - Simulated external and internal threat activity

### Targets

- **srv-01**
  - Exposed services used for testing
- **srv-ids**
  - Monitored destination to validate IDS visibility
- **admin-ws**
  - Used to simulate legitimate administrative activity

---

## Scenario 1: Network Reconnaissance (Nmap)

### Description

This scenario simulates an early-stage reconnaissance phase where an attacker performs network discovery and service enumeration from a Kali Linux host.

### Tools Used
- `nmap`

---

### Environment

- **Attacker:** Kali Linux (`10.0.2.50`)
- **Target Network:** Workstation LAN (`192.168.1.0/24`)
- **Detection Stack:** Suricata IDS → Splunk SIEM

---

### Step 1 - Host Discovery (Ping Sweep)

Identify live hosts on the target network:

`sudo nmap -sn 192.168.1.0/24 -oN nmap_s1_host_discovery.txt`

### Observed Behavior

- ICMP echo requests across the subnet
- ARP / ICMP traffic visible to Suricata
- Low-noise but clear reconnaissance pattern

See below screenshots:<br>
 [Nmap_Host_Discovery](Screenshots/Scenario_01/Nmap_Host_Discovery.png)<br>
 [Recent_suspicious_alert_scan](Screenshots/Scenario_01/Recent_suspicious_alert_scan.png)<br>
 [Suspicious_over_time_scan](Screenshots/Scenario_01/Suspicious_over_time_scan.png)

 --

### Step 2 - TCP SYN Port Scan (Subnet)

Perform a classic SYN scan to identify open ports:

`sudo nmap -sS -T3 -Pn 192.168.1.0/24 -oN nmap_s1_synscan_subnet.txt`

### Observed Behavior

- Multiple connection attempts to target hosts
- Port probing and service discovery patterns
- Increased network traffic volume

---

### Detection Results

- Suricata generated IDS events related to scanning behavior
- Events were ingested into Splunk in real time
- Alerts appeared in:
  - IDS Network Threat Dashboard
  - SOC Overview Dashboard
  
In contrast to the earlier baseline where all alerts were classified as benign sensor noise, this scenario demonstrates a successful shift to actionable detection: the Nmap reconnaissance traffic is now correctly flagged as suspicious, validating both the tuning effort and the effectiveness of the detection pipeline.
  
See below screenshots:<br>
 [Nmap_synscan](Screenshots/Scenario_01/Nmap_synscan.png)<br>
 [Recent_suspicious_alert_synscan](Screenshots/Scenario_01/Recent_suspicious_alert_synscan.png)<br>
 [Top_Alert_suspicious_only_after_scan](Screenshots/Scenario_01/Top_Alert_suspicious_only_after_scan.png)<br>
 [Noise_vs_Suspicious_after_scan](Screenshots/Scenario_01/Noise_vs_Suspicious_after_scan.png)

Search in Splunk:<br>
 [Splunk_Search_Suricata_Events](Screenshots/Scenario_01/Splunk_Search_Suricata_Events.png)<br>
 [Splunk_Search_Scan_Related_Signatures](Screenshots/Scenario_01/Splunk_Search_Scan_Related_Signatures.png)
 
----

### SOC Interpretation

- Reconnaissance activity is a common **early-stage attack indicator**
- Detection confirmed correct IDS placement and traffic visibility
- Events validated the Suricata → Splunk ingestion pipeline

---
<br>

## Scenario 2: SSH Brute-Force Attack Simulation

### Description

A credential brute-force attack was simulated from the Kali Linux VM against an SSH service exposed within the lab environment. 

This scenario represents a common post-reconnaissance attack technique used to gain unauthorized access through weak or reused credentials.

! All actions were executed in a closed lab environment owned and managed by me, strictly for defensive learning purposes only.
These techniques must not be used against real systems, accounts, or networks. !

---

### Tools Used
- `hydra`

---

### Environment

- **Attacker:** Kali Linux (`10.0.2.50`)
- **Target Network:** admin-ws (`192.168.1.10`)
- **Detection Stack:** Suricata IDS → Splunk SIEM
- **Target Service:** SSH (TCP/22)

---

### Attack Methodology

- Targeted SSH authentication service
- Multiple login attempts using a password list
- Rapid sequential connection attempts from a single source IP


Password authentication was intentionally allowed temporarily to simulate a common misconfiguration found in legacy or poorly hardened systems. Although SSH password authentication was enabled for testing purposes, other security controls remained in place, allowing detection of brute-force behavior without exposing the system to real risk.

---

### Step 1 - Prepare a Controlled password list (for the simulation)

Create a small password list (use lab-only demo passwords)

See below screenshot:<br>
[Hydra_attack_01_password_list](Screenshots/Scenario_02/Attack_01/Hydra_attack_01_password_list.png)

--

### Step 2 - Launch the Brute-Force Simulation with Hydra

Below is a template that shows the components Hydra needs. I replace real arguments with placeholders so this is conceptual rather than a copy-paste ready attack.

`sudo hydra [user-option] [pass-option] [control-flags] TARGET -s PORT MODULE "MODULE_PARAMS"`

For security reasons I don’t publish a copy-able attack line giving an exact runnable command. The template above is sufficient to illustrate the structure used in the isolated lab environment.

### Placeholders breakdown:

[user-option] — single user (-l alice) or user list (-L /path/userlist.txt)
[pass-option] — single password (-p Pa$$w0rd) or password list (-P /path/passwords.txt)
TARGET — lab host (e.g., 192.168.1.10)
-s PORT — target port (80/443 for web, 22 for SSH, etc.)
MODULE — the protocol module (e.g., http-post-form for typical web forms)
"MODULE_PARAMS" — module-specific parameters: the path to the form, names of username/password fields, and a failure condition that tells Hydra when to continue vs stop.

See below screenshots:<br>
 [Hydra_attack_01](Screenshots/Scenario_02/Attack_01/Hydra_attack_01.png)<br>
 [Hydra_attack_01_result](Screenshots/Scenario_02/Attack_01/Hydra_attack_01_result.png)
 
---

### Observed Behavior

- Repeated failed authentication attempts
- High-frequency connection attempts to the SSH service
- Clear deviation from normal administrative login patterns

See below screenshots:<br>
[admin_ws_ssh_brute_force_failed_attempt_auth_log](Screenshots/Scenario_02/Attack_01/admin_ws_ssh_brute_force_failed_attempt_auth_log.png)

---

### Detection Results

- Suricata generated alerts related to abnormal connection behavior
- Authentication failures were logged and indexed in Splunk
- Events were visible across:
  - Authentication Monitoring Dashboard
  - SOC Overview Dashboard
  - IDS Network Threat Dashboard
  
See below screenshots:<br>
 [splunk_auth_failure_over_time_after_hydra_attack](Screenshots/Scenario_02/Attack_01/splunk_auth_failure_over_time_after_hydra_attack.png)<br>
 [splunk_ids_alert_after_hydra_attack](Screenshots/Scenario_02/Attack_01/splunk_ids_alert_after_hydra_attack.png)<br>
 [splunk_suspicious_alert_over_time_after_hydra_attack](Screenshots/Scenario_02/Attack_01/splunk_suspicious_alert_over_time_after_hydra_attack.png)
 
Confirmed also via Network and Host telemetry search:<br>
 [Splunk_search_after_hydra_attack](Screenshots/Scenario_02/Attack_01/Splunk_search_after_hydra_attack.png)<br>
 [Splunk_search_02_after_hydra_attack](Screenshots/Scenario_02/Attack_01/Splunk_search_02_after_hydra_attack.png)
 
---

### SOC Interpretation
- Repeated authentication failures from a single source are a strong indicator of brute-force activity
- Correlation between:
  - Network-level alerts (Suricata)
  - Host-level authentication logs
- Demonstrates the value of combining IDS and authentication telemetry

---

### Analyst Response Example
- Identify the attacking source IP
- Review failed vs successful authentication ratios
- Confirm absence of successful compromise
- Recommend containment actions:
  - Temporary IP blocking at the firewall
  - Credential review for targeted accounts
  - Alert escalation if attempts persist
 
---

## Scenario 2 (Follow-up): Successful SSH Brute-Force Compromise

### Description

Following the initial brute-force attempt that resulted only in authentication failures, a second controlled test was performed in which the brute-force activity successfully led to an unauthorized SSH login.

The target system was intentionally temporarily configured with weak credentials and no ssh hardening to simulate a realistic, vulnerable environment.

---

### Step 1 - Append the Controlled Password List

Append the existing password list with the correct password of the target machine

See below screenshot:<br>
[password_list_appended](Screenshots/Scenario_02/Attack_02/password_list_appended.png)

### Step 2 - Launch the Brute-Force Simulation with Hydra

See below screenshot:<br>
[hydra_attack_02](Screenshots/Scenario_02/Attack_02/hydra_attack_02.png)

---

## Observed Behavior

### Host-Level Activity
- Multiple failed SSH authentication attempts
- A subsequent **successful SSH authentication**
- Establishment of an SSH session originating from the attacker IP

### Network-Level Activity
- Repeated TCP connection attempts to port 22
- Short-lived connections consistent with automated login attempts
- Sustained abnormal access patterns from a single source IP

See below screenshot:<br>
[successfull_bruteforce](Screenshots/Scenario_02/Attack_02/successfull_bruteforce.png)

---

## Detection Results

### Authentication Telemetry
- SSH authentication logs recorded:
  - Numerous `Failed password` events
  - A later `Accepted password` event from the same source IP
- Successful authentication confirms unauthorized access

These logs were indexed into Splunk and correlated by time and source.

### IDS / Network Telemetry
- Suricata observed abnormal SSH connection patterns
- Network alerts provided context for the authentication activity
- IDS visibility confirmed repeated access attempts preceding compromise

See below screenshot:<br>
[ssh_alerts](Screenshots/Scenario_02/Attack_02/ssh_alerts.png)

---

### SIEM Correlation (Splunk)

Splunk correlation enabled:
- Identification of the attacking IP address
- Confirmation of abnormal authentication behavior
- Validation of unauthorized access through accepted login events

Relevant events appeared across:
- **Authentication Monitoring Dashboard**
- **SOC Overview Dashboard**
- **IDS Network Threat Dashboard**

See below screenshots:<br>
[auth_failure_over_time_second_attack](Screenshots/Scenario_02/Attack_02/auth_failure_over_time_second_attack.png)<br>
[suspicious_alert_raising_over_time](Screenshots/Scenario_02/Attack_02/suspicious_alert_raising_over_time.png)<br>
[alert_level_2_increase](Screenshots/Scenario_02/Attack_02/alert_level_2_increase.png)

---

## SOC Interpretation

- Successful brute-force attacks are often the result of:
  - Weak or reused passwords
  - Exposed services without access restrictions
  - Insufficient hardening of authentication mechanisms
- Correlating authentication success with abnormal access patterns is critical for accurate incident classification

This scenario highlights how poor security hygiene can directly lead to system compromise.

---

## Security Lessons Learned

- Strong password policies significantly reduce brute-force success
- Restricting SSH access (IP allowlisting, key-based auth) lowers attack surface
- Monitoring **authentication outcomes** is as important as monitoring attack attempts
- Defense-in-depth is essential to prevent single-point failures

---

## Analyst Response Example

In a production environment, the following actions would be recommended:

- Immediate containment of the source IP
- Forced credential rotation for compromised accounts
- Review of system access logs for post-compromise activity
- Hardening of SSH configuration:
  - Disable password authentication
  - Enforce key-based access
  - Apply rate limiting or fail2ban
- Escalation according to incident severity

---

### Why This Scenario Matters

Credential-based attacks remain one of the most common initial access techniques.  
This scenario validates the lab’s ability to:

- Detect brute-force behavior
- Correlate network and authentication events
- Support real-world SOC investigation workflows

---

## MITRE ATT&CK Mapping

The attack scenarios executed in this lab align with the following MITRE ATT&CK techniques:

### Scenario 1: Network Reconnaissance (Nmap)

- **T1046 – Network Service Discovery**
  - Active scanning of the target subnet to identify live hosts and exposed services.
  - Observed through ICMP sweeps and TCP SYN probes.
  
   `https://attack.mitre.org/techniques/T1046/`

- **T1595 – Active Scanning**
  - Systematic probing of network infrastructure prior to exploitation.
  - Represented an early-stage reconnaissance phase commonly seen in real intrusions.
  
   `https://attack.mitre.org/techniques/T1595/`

---

### Scenario 2: Successful SSH Brute-Force Attack

- **T1110 – Brute Force**
  - Automated SSH authentication attempts using multiple credentials.
  - Evidenced by repeated failed logins followed by a successful authentication.
  
   `https://attack.mitre.org/techniques/T1110/`

- **T1078 – Valid Accounts**
  - Successful SSH access obtained using compromised credentials.
  - Demonstrates abuse of legitimate authentication mechanisms for unauthorized access.
  
   `https://attack.mitre.org/techniques/T1078/`


Together, these techniques illustrate a realistic attack progression from **reconnaissance** to **initial access**, reinforcing the importance of detecting early-stage activity before compromise occurs.

---

## Key Takeaways

- IDS and SIEM integration successfully captured multiple attack phases
- Alert classification prevented noise from overwhelming dashboards
- Controlled attack simulations provided realistic security telemetry without exposing the lab to unnecessary risk
- The lab reflects realistic SOC conditions, including imperfect executions

This section bridges the gap between **infrastructure setup** and **security operations analysis**.
