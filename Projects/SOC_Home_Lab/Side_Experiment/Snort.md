## 🐷 Snort IDS — Manual Rule Testing and Traffic Validation

In addition to the main IDS deployment based on Suricata, I ran a small side experiment with **Snort** to strengthen my understanding of **signature-based IDS detection** at a lower level. This experiment was not integrated into the SIEM pipeline; it was intentionally kept lightweight and focused on learning, rule logic, and real-time validation.

---

### Objective

This side experiment aimed to:

- Understand how a signature-based IDS evaluates traffic and triggers alerts
- Practice writing and testing custom detection rules
- Validate alert behavior using controlled, manually generated traffic
- Observe how quickly “noise” can appear without careful scoping/tuning

---

### Network Scope Definition (HOME_NET)

Snort was configured with internal network variables aligned with the lab segments. Two internal networks were defined to represent the workstation and server LANs:

- `192.168.1.0/24`
- `10.0.3.0/24`

All other traffic was considered external (`EXTERNAL_NET any`). This made it easier to reason about what should be treated as “internal” vs “external” during testing.

See below screenshot:<br>
[Network_scope_definition](Screenshots/Snort/Network_scope_definition.png)

---

### Custom Local Rules (local.rules)

To validate detection, I added a few simple **local rules**:

1) **ICMP detection**  
A baseline rule to trigger alerts on ICMP traffic (e.g., ping). This confirmed Snort was properly listening on the correct interface and processing traffic.

2) **HTTP GET detection**  
A rule inspecting TCP traffic to port 80 that triggers when `GET` is detected. This validated basic payload inspection for HTTP requests.

3) **Suspicious User-Agent detection (curl)**  
A rule that detects HTTP headers containing the string `curl`, demonstrating how IDS rules can flag scripted or automated tooling that deviates from typical browser user agents.

These rules were intentionally straightforward so the signal would be obvious during testing.

See below screenshot:<br>
[snort_local_rules](Screenshots/Snort/snort_local_rules.png)

---

### Live Validation (Console Mode)

Snort was executed manually in **console mode** on the monitoring interface to observe alerts in real time.

Test traffic was generated from another host using simple tools such as:

- ICMP ping requests (to trigger the ICMP alert)
- HTTP requests (to trigger GET/User-Agent rules)

As soon as test traffic was generated, Snort produced the expected alerts, confirming:

- Correct interface monitoring
- Correct `HOME_NET` scope
- Rules were loaded and active
- Alerts were firing in real time as expected

See below screenshot:<br>
[icmp_test](Screenshots/Snort/icmp_test.png)

---

### Observations and Learnings

This small experiment reinforced several practical insights:

- **Rule writing improves protocol understanding** (ICMP/HTTP behavior becomes much clearer when you write the detection logic yourself).
- **Noise happens fast** if rules are too broad or not scoped carefully.
- Console mode is excellent for **learning and debugging**, while dashboards/SIEM workflows are better for operational monitoring and triage.
- The exercise helped build intuition around **signal vs noise**, which directly translated to later IDS tuning work in the lab.

---

### Relation to the Main Lab

Snort was not selected as the primary IDS in the final lab. **Suricata** remained the main IDS due to stronger JSON logging and easier ingestion into Splunk.  
However, this Snort side test complemented the project by providing hands-on experience with:

- Signature mechanics
- Rule conditions and matching
- Alert generation behavior
- Early tuning considerations

---

### Why This Matters

Being able to reason about IDS alerts at the rule level matters in SOC and detection-focused roles. This experiment demonstrates the ability to go beyond dashboards and actively understand **how alerts are generated**, why they trigger, and how rule scope impacts noise.
