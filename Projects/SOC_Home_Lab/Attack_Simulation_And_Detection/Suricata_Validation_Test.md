# Suricata Validation Test – TestMyNIDS

## Purpose

Before executing any attack simulations, a controlled validation test was performed to confirm that:

- Suricata was correctly deployed and actively inspecting traffic
- IDS alerts were generated as expected
- Events were successfully ingested into Splunk
- Dashboards reflected real-time detection activity

This step ensures that the detection pipeline was fully operational before running more complex attack scenarios.

---

## Test Description

The TestMyNIDS endpoint used in this lab is provided by the open-source project maintained by 3CORESec and is designed specifically to validate IDS/IPS detection capabilities in a safe and controlled manner.

Source: `https://github.com/3CORESec/testmynids.org`

The test simulates a malicious server response that returns privileged execution output, which should be detected by Suricata as suspicious behavior.

---

## Environment

- **Source Host:** Admin Workstation
- **Target:** `http://testmynids.org`
- **Detection Stack:**  
  - Suricata IDS  
  - Splunk SIEM  

---

## Test Execution

The following command was executed from the admin workstation:

`curl http://testmynids.org/uid/index.html`

---

## Expected Result

The HTTP response deliberately returns:

`uid=0(root) gid=0(root) groups=0(root)`

This response is intentionally crafted to trigger IDS rules related to suspicious command execution or information disclosure.

---

## Observed Behavior

- The HTTP request completed successfully
- Suricata generated alerts immediately after the request
- Alerts were classified under signatures related to suspicious server responses
- Multiple events were generated due to repeated response patterns

See below screenshots:<br>
[testmynids_cli](Screenshots/Testmynids/testmynids_cli.png)<br>
[testmynids_alerts](Screenshots/Testmynids/testmynids_alerts.png)

---

## Detection Results

### Suricata

Suricata generated alerts with signatures similar to:

`GPL ATTACK_RESP ONSE id check returned root`

These alerts indicate detection of abnormal or potentially malicious response content.

### Splunk

Events were ingested into the suricata index

Alerts were visible in:

- IDS Network Threat Dashboard
- SOC Overview Dashboard

Source IP, destination IP, protocol, and signature details were correctly parsed.

---

## SOC Interpretation

This test confirms that:

- Suricata is correctly positioned in the network path
- IDS signatures are actively firing
- The log ingestion pipeline (Suricata → Splunk) is functional
- Dashboards reflect detection events in near real time

This validation step establishes a trusted baseline before executing reconnaissance and attack simulations.

---

## Why This Test Matters

Running a known-good IDS test before attack simulations is a best practice because it:

- Eliminates uncertainty about sensor visibility
- Prevents misattribution of missing alerts to attack tooling
- Confirms correct parsing, indexing, and visualization
- Provides confidence in subsequent detection results
