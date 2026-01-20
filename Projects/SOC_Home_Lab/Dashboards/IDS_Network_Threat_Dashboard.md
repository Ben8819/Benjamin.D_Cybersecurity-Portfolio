# IDS Network Threat Dashboard

## Purpose

The **IDS Network Threat Dashboard** provides a focused, analyst-oriented view of **network intrusion activity detected by Suricata**.  
Its primary objective is to **separate actionable security signals from sensor noise**, enabling efficient SOC triage and investigation.

After alert refinement and noise filtering, this dashboard intentionally prioritizes **signal quality over alert volume**, even if that results in panels temporarily showing no data.

---

## Data Sources

- **Suricata IDS**
  - Alert events
  - Signature metadata
  - Severity levels
  - Source and destination context
- **Splunk**
  - Centralized log ingestion
  - Event correlation and filtering
  - Visualization and time-based analysis

---

## Alert Refinement Strategy

Before refinement, the environment generated a large volume of **benign Suricata stream alerts**, such as:

- TCP retransmissions
- Invalid timestamps
- Protocol-level anomalies caused by normal traffic

See below screenshots:<br>
[Alerts_Before_Refinement](Screenshots/IDS_Network_Threat_Dashboard/Alerts_Before_Refinement.png)<br>
[Alerts_Before_Refinement_02](Screenshots/IDS_Network_Threat_Dashboard/Alerts_Before_Refinement_02.png)

The following refinement actions were applied:

- Classification of known noisy signatures as **Benign (Sensor Noise)**
- Exclusion of non-actionable `STREAM` and low-value protocol alerts
- Dashboard logic updated to:
  - Focus on **Suspicious alerts only**
  - Explicitly surface when **no threats are present**

This approach mirrors real-world SOC practices, where reducing alert fatigue is critical.

---

The SPL queries used to build and validate this dashboard panels are documented as screenshots in the dedicated [SPL/](Screenshots/IDS_Network_Threat_Dashboard/SPL) folder.

## Dashboard Panels

### 1. Suspicious Alerts Over Time

**Description**
- Time-series visualization of alerts classified as **Suspicious**
- Benign alerts are intentionally excluded

**Security Value**
- Highlights temporal spikes caused by:
  - Reconnaissance activity
  - Exploit attempts
  - Attack simulations
- Flat baselines indicate normal network behavior

**Current State**
- Only **Benign (Sensor Noise)** activity observed
- No confirmed malicious activity detected during the selected timeframe

See below screenshot:<br>
[suspicious_alert_over_time](Screenshots/IDS_Network_Threat_Dashboard/suspicious_alert_over_time.png)

---

### 2. Alert Classification (Noise vs Suspicious)

**Description**
- Distribution of IDS alerts by classification:
  - `Benign (Sensor Noise)`
  - `Suspicious`

**Security Value**
- Immediate visibility into:
  - Signal-to-noise ratio
  - Effectiveness of alert tuning

**Current State**
- 100% of alerts classified as **Benign (Sensor Noise)**
- Confirms successful suppression of false positives

See below screenshot:<br>
[alert_classification_noise_vs_suspicious](Screenshots/IDS_Network_Threat_Dashboard/alert_classification_noise_vs_suspicious.png)

---

### 3. Recent Suspicious Alerts Table

**Description**
- Tabular view of the most recent **suspicious IDS alerts**
- Fields include:
  - Timestamp
  - Source IP and port
  - Destination IP and port
  - Protocol
  - Alert signature
  - Severity

**Security Value**
- Primary triage panel for SOC analysts
- Used for rapid investigation and pivoting

**Current State**
- **No search results returned**
- Expected behavior when no suspicious activity is detected

See below screenshot:<br>
[recent_suspicious_alert](Screenshots/IDS_Network_Threat_Dashboard/recent_suspicious_alert.png)

---

### 4. Top Alert Signatures (Suspicious Only)

**Description**
- Ranking of the most frequent **suspicious IDS signatures**
- Benign signatures are excluded by design

**Security Value**
- Identifies recurring attack patterns
- Helps guide detection engineering and rule tuning

**Current State**
- **No search results returned**
- Indicates absence of recurring malicious signatures

See below screenshot:<br>
[top_alert_signature_suspicious](Screenshots/IDS_Network_Threat_Dashboard/top_alert_signature_suspicious.png)

---

## Analyst Interpretation

This dashboard demonstrates a realistic SOC outcome:

- The IDS is **active and ingesting traffic**
- Alert noise has been successfully identified and filtered
- No suspicious network activity is currently present
- Dashboards remain meaningful even when no alerts are raised

In operational environments, **silence is often a healthy signal**.

---

## Limitations and Future Improvements

**Current Limitations**
- Limited attack diversity due to lab scope
- Absence of continuous real-world traffic

**Planned Enhancements**
- Additional simulated attack scenarios
- Severity-based alert escalation
- Correlation with firewall and authentication events
- Automated threat summaries

---

## Key Takeaway

A dashboard showing **no suspicious alerts** is not a failure.

It demonstrates:
- Effective alert tuning
- Reduced false positives
- SOC-ready visualization
- Analyst-focused design

This IDS Network Threat Dashboard reflects a **mature detection posture** aligned with real-world SOC operations.
