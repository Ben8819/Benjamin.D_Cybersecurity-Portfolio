# Firewall Rules Design

## Purpose of This Section

This document explains the **logic and intent** behind the firewall rules implemented on pfSense.  
Rather than listing raw rule screenshots, this section focuses on **why** rules exist, **how they are grouped**, and **what security problems they solve**.

This approach reflects how firewall policies are documented in professional environments and makes the configuration understandable for SOC analysts and security engineers.

---

## Firewall Rule Strategy Overview

The firewall rules follow these core principles:

- **Default deny** on all interfaces
- **Least privilege** access between networks
- **Explicit management paths**
- **Clear separation between trusted and untrusted zones**
- **Temporary rules for testing only**

Each interface enforces policies based on the **trust level** of the network it represents.

---

## Interface-Based Rule Design

### Aliases

Firewall aliases were used to group networks, hosts, and ports in order to keep rules readable, scalable, and easier to maintain as the lab evolved.

See below screenshots:<br>
[firewall_ip_aliases]<br>
[firewall_port_aliases]

---


### 1. LAN Interface (Management / User Network)

The LAN interface represents a trusted internal user network, but **not all LAN hosts are administrators**.

#### Allowed Traffic
- `admin-ws` → pfSense  
  - Web GUI (HTTPS)
  - SSH access
- `admin-ws` → SERVER_LAN  
  - Splunk Web interface
  - Management services
- LAN hosts → Internet  
  - Outbound access via NAT (updates, browsing)

#### Denied Traffic
- LAN hosts → SERVER_LAN (unauthorized services)
- LAN hosts → SSH access to internal servers
- Any lateral movement not explicitly required


Only the administrative workstation should be able to manage infrastructure components.  
Other LAN hosts behave as standard user endpoints.

See below screenshot:<br>
[firewall_lan_rules]

---

### 2. SERVER_LAN Interface (Security / Infrastructure Network)

This interface hosts critical security infrastructure and is treated as a **restricted zone**.

#### Allowed Traffic
- `admin-ws` → `srv-ids`  
  - Splunk Web
  - Administrative access
- Servers → Splunk  
  - Log forwarding
- SERVER_LAN → Internet  
  - Updates and package retrieval (via NAT)

#### Denied Traffic
- Unrestricted inbound access from LAN
- Any access from KALI_LAN by default
- Lateral server-to-server access unless required


Security tooling must be accessible for administration but protected from unnecessary exposure.

See below screenshot:<br>
[firewall_server_lan_rules]

---

### 3. KALI_LAN Interface (Attacker Network)

This interface represents an **untrusted or compromised internal segment**.

#### Default Policy
- **All traffic denied** to internal networks

#### Temporary Allowed Traffic (Testing Only)
- Controlled access to specific targets
- Enabled only during attack simulation exercises
- Disabled immediately after testing


The attacker network is intentionally isolated to:
- Prevent accidental damage
- Avoid contaminating normal traffic
- Maintain clean detection baselines

See below screenshot:<br>
[firewall_kali_lan_rules]

---

## Management Access Control

Management access is strictly enforced:

- Only `admin-ws` can:
  - Access pfSense management interfaces
  - Administer Splunk
  - Manage IDS configuration
- No management access from:
  - KALI_LAN
  - Other LAN hosts
  - SERVER_LAN hosts

This reflects real-world security practices where management planes are isolated.

---

## Outbound Traffic and NAT

Outbound access is handled through pfSense NAT:

- Internal networks are translated to the WAN interface
- No inbound connections from the internet are allowed
- Egress is permitted for:
  - System updates
  - Tool installation
  - Controlled testing

NAT rules are kept minimal and explicit to avoid unintended exposure.

---

## Logging and Visibility

Firewall rules are configured to provide visibility rather than silent blocking:

- Important rule matches are logged
- Logs are forwarded to Splunk
- Firewall data is correlated with:
  - IDS alerts
  - Host authentication logs
  - SIEM dashboards

This allows analysts to understand **why** traffic was allowed or blocked.

---

## Temporary Rules and Change Discipline

All temporary rules used for testing follow strict guidelines:

- Clearly labeled and documented
- Enabled only for the duration of testing
- Removed immediately after validation

This demonstrates operational discipline and change management awareness.

---

## Security Trade-Offs

Some trade-offs were intentionally accepted:

- pfSense operates as a firewall, not an inline IPS
- Detection is prioritized over prevention
- Noise is handled at the SIEM layer rather than through overly aggressive blocking

These decisions reflect realistic SOC environments where visibility and analysis are critical.

---

## Summary

The firewall rule design:

- Enforces clear trust boundaries
- Protects critical infrastructure
- Supports IDS and SIEM visibility
- Enables controlled attack simulations
- Reflects enterprise security practices

This rule set provides a strong foundation for detection, monitoring, and incident analysis throughout the lab.
