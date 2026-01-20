# Security Information and Event Management (Splunk)

## Purpose of Splunk in the Lab

Splunk is used as the **central SIEM platform** for this lab.  
Its role is to collect, normalize, correlate, and visualize security-relevant data from multiple sources in order to support SOC-style monitoring and investigation workflows.

Splunk was chosen because it:
- Is widely used in enterprise SOC environments
- Supports both host-based and network-based telemetry
- Enables flexible search, correlation, and dashboards
- Reflects real-world analyst tooling and workflows

The goal is **detection, analysis, and visibility**, not automated response.

---

## Data Sources Integrated into Splunk

The SIEM ingests data from multiple layers of the environment:

### Network-Based Telemetry
- **Suricata IDS alerts** (EVE JSON format)
- Firewall logs from pfSense (syslog)

### Host-Based Telemetry
- Authentication logs (e.g. SSH login attempts)
- System logs from monitored Linux hosts

These sources provide complementary perspectives:
- Network behavior
- Host authentication activity
- Perimeter enforcement context

See below screenshots:<br>
[eve_json](Screenshots/eve_json.png)<br>
[pfsense_syslog](Screenshots/pfsense_syslog.png)

---

## Splunk Architecture in the Lab

### Splunk Enterprise (Indexer / Search Head)
- Deployed on: `srv-ids` (`10.0.3.20`)
- Roles:
  - Indexing incoming data
  - Storing events
  - Providing search and dashboard capabilities

### Splunk Universal Forwarders
Installed on monitored hosts to forward logs securely and efficiently to the SIEM.

Forwarders are responsible for:
- Monitoring log files
- Sending events to Splunk over TCP
- Minimizing performance impact on endpoints

See below screenshot:<br>
[splunk_listening_port](Screenshots/splunk_listening_port.png)

---

## Index Design

Indexes were created to logically separate data sources and simplify analysis.

Examples of index usage:
- Host authentication and system logs
- IDS alerts
- Firewall and network events

Separating data into indexes allows:
- Cleaner searches
- More efficient dashboards
- Clear ownership of log sources

Index naming reflects the **source and purpose** of the data rather than arbitrary grouping.

See below screenshot:<br>
[index_list](Screenshots/index_list.png)


---

## Log Ingestion and Normalization

### Ingestion Methods
- **Forwarded logs** via Splunk Universal Forwarder
- **Syslog inputs** for network devices
- **File monitoring** for IDS output

### Normalization Approach
- Source types are used to identify log formats
- Fields such as:
  - `src_ip`
  - `dest_ip`
  - `user`
  - `event_type`
  are extracted where applicable

Normalization is kept intentionally simple to:
- Preserve raw visibility
- Avoid hiding anomalies
- Allow manual analyst interpretation

See below screenshot:<br>
[source_type](Screenshots/source_type.png)

---

## Analyst Workflow in Splunk

Splunk is used to support a typical SOC workflow:

1. **Detection**
   - Alerts from Suricata and authentication logs appear as events
2. **Triage**
   - Events are filtered by time, source IP, and severity
3. **Correlation**
   - Network alerts are correlated with host authentication activity
4. **Investigation**
   - Timelines are reconstructed
   - Patterns are analyzed (e.g. repeated failures)
5. **Visualization**
   - Dashboards provide situational awareness

The emphasis is on **understanding behavior**, not reacting to single alerts.

---

## Dashboards and Monitoring

Dashboards were built to provide:

- Authentication activity visibility
- SOC-style overview of security events
- IDS and network threat monitoring

Each dashboard focuses on a specific operational question rather than raw data volume.

Detailed dashboard descriptions are provided in the dedicated [`Dashboards/`](SOC_Home_Lab/Dashboards) section of this project.

---

## Alert Context and Severity Handling

Alert severity is treated as **contextual**, not absolute.

Key principles:
- Low or medium severity alerts can still indicate real threats
- Patterns over time are more important than individual events
- Noise is expected and documented

This reflects real SOC operations where analysts rely on correlation and judgment.

---

## Limitations and Design Decisions

Several constraints were accepted intentionally:

- No automated response or SOAR integration
- Minimal enrichment to preserve raw visibility
- Some background noise due to virtualized networking
- Manual correlation instead of prebuilt detection content

These choices prioritize learning, transparency, and analyst reasoning.

---

## Summary

Splunk serves as the analytical core of the lab by:

- Centralizing logs from multiple layers
- Enabling correlation of network and host activity
- Supporting SOC-style investigation workflows
- Providing dashboards for operational visibility

The SIEM implementation emphasizes **defensive analysis**, **pattern recognition**, and **realistic security monitoring practices**.
