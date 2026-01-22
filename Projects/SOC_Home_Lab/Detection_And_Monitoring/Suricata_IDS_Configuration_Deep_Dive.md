# 👁️ Suricata IDS – Configuration Deep Dive

This section adds extra technical detail about the Suricata IDS configuration used in the lab. It focuses on the reasoning behind key settings (network variables, rule loading, validation, capture mode, and BPF filtering)

---

## 1) Network scope definition (HOME_NET / EXTERNAL_NET)

Suricata was explicitly configured with the internal networks it should treat as “protected.” Being specific here improves alert relevance and reduces false positives.

### Configuration (example)

  address-groups:<br>
    HOME_NET: "[192.168.1.0/24,10.0.3.0/24]"<br>
    EXTERNAL_NET: "!$HOME_NET"

What this does:

- HOME_NET defines the internal ranges that matter in this lab:

- 192.168.1.0/24 → workstation LAN (Admin workstation segment)

- 10.0.3.0/24 → server / IDS monitoring segment

- EXTERNAL_NET becomes “everything that is not HOME_NET”

- This allows signatures to correctly detect “external → internal” patterns, which are typically higher priority.

See below screenshot:<br>
[home_net_config](Screenshots/home_net_config.png)

---

## 2) Rule loading strategy (managed rules + local rules)

Suricata was configured to load:

- a managed ruleset (updated through tooling), and

- a local rules file for controlled tests and custom detections.

Configuration (rule path + files)

default-rule-path: /var/lib/suricata/rules

rule-files:
  - suricata.rules
  - local.rules
    
--

### Why this approach

suricata.rules

- Represents the “realistic SOC feed” (community/threat rules maintained externally)

local.rules

- Enables quick test rules (ICMP / HTTP / header matching)

- Supports learning and validation without polluting the full ruleset

See below screenshot:<br>
[rules_path_config](Screenshots/rules_path_config.png)

---

## 3) Local rules used for testing & learning

Local rules were added to confirm the end-to-end pipeline works:
traffic → Suricata inspection → alert generation → (optional) SIEM ingestion.

### Example local rules

#### Detect ICMP ping
`alert icmp any any -> any any (msg:"ICMP Test Alert"; sid:1000001; rev:1;)`

#### Detect HTTP GET requests
`alert tcp any any -> any 80 (msg:"HTTP GET Detected"; content:"GET"; sid:1000002; rev:1;)`

#### Detect suspicious User-Agent strings (curl)
`alert tcp any any -> any 80 (msg:"Suspicious User-Agent"; content:"curl"; http_header; sid:1000003; rev:1;)`

### What this validates

- ICMP rule confirms basic packet visibility.
  
- HTTP GET rule confirms payload inspection works.
  
- User-Agent rule confirms Suricata can parse HTTP headers and match on them.

---

## 4) Configuration validation (before running)

Before starting Suricata in monitoring mode, the configuration was validated to avoid silent failures.

See below screenshots:<br>
[suricata_icmp_test](Screenshots/suricata_icmp_test.png)

### Validation command

`sudo suricata -T -c /etc/suricata/suricata.yaml`

See below screenshot:<br>
[suricata_config_test](Screenshots/suricata_config_test.png)

### Expected result

- Config loads successfully
  
- Rules load successfully
  
- Suricata exits cleanly after the test

This is a standard operational practice in real environments before deploying changes.

---

## 5) Packet capture method (AF_PACKET)

Suricata used AF_PACKET for high-performance capture on Linux.

### Configuration (example)

af-packet:
  - interface: ens5<br>
    threads: auto
	
See below screenshot:<br>
[suricata_config_interface](Screenshots/suricata_config_interface.png)

### Why AF_PACKET

- Efficient for mirrored/SPAN traffic capture
  
- Supports multi-threading (threads: auto)
  
- Common in IDS deployments where traffic volume can spike

---

## 6) Noise reduction using BPF filtering

To reduce sensor noise and avoid self-generated traffic causing alerts, a BPF filter was added.

### Configuration (example)

af-packet:
  - interface: ens5<br>
    bpf-filter: "not host 10.0.3.20"
	
See below screenshot:<br>
[suricata_config_interface_filtering](Screenshots/suricata_config_interface_filtering.png)

### What this does

- Excludes traffic to/from the IDS/SIEM host (example: 10.0.3.20)

Helps prevent:

- “IDS alerts about its own log traffic”

- feedback loops / self-noise

- Improves signal-to-noise ratio in Splunk dashboards

----

## Key takeaways

- Accurate HOME_NET/EXTERNAL_NET definitions improve detection relevance.

- Loading both managed rules and local.rules creates a realistic SOC setup + a safe learning sandbox.

- suricata -T validation prevents broken deployments.

- AF_PACKET is a practical capture method for Linux sensors.

- BPF filtering is a simple but powerful way to reduce noise and keep dashboards meaningful.
