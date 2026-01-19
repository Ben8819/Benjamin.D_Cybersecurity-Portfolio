# pfSense Design and Network Security

## Purpose of pfSense in the Lab

pfSense is used as the **central security control point** of the lab.  
It acts as the perimeter firewall, router, and traffic enforcement device for all internal networks.

The goal of using pfSense in this project is to demonstrate:

- Network segmentation and trust boundaries
- Firewall rule design following a least-privilege approach
- Controlled management access
- Separation between user, server, and attacker environments
- Realistic SOC-facing network visibility

pfSense was chosen because it is widely used in enterprise and SOC environments and provides transparent, auditable firewall behavior.

---

## Network Interfaces and Roles

pfSense connects all network segments and routes traffic between them.

### WAN Interface
- **Role:** Internet access
- **Function:** Outbound connectivity for internal networks via NAT
- **Security posture:** No inbound access allowed

--

### LAN Interface (Management / User Network)
- **Subnet:** `192.168.1.0/24`
- **Gateway:** `192.168.1.1`
- **Primary host:** `admin-ws` (`192.168.1.10`)

This interface represents a standard internal user environment.

It is the **only network** allowed to:
- Access pfSense management interfaces (GUI, SSH)
- Perform administrative actions across the lab
- Manage security tooling (IDS, SIEM)

--

### SERVER_LAN Interface (Security / Infrastructure Network)
- **Subnet:** `10.0.3.0/24`
- **Gateway:** `10.0.3.1`
- **Primary host:** `srv-ids` (`10.0.3.20`) , `srv-01` (10.0.3.10)

This interface hosts critical security infrastructure:
- Suricata (IDS)
- Splunk Enterprise (SIEM)

It is treated as a **protected zone**, accessible only through explicit firewall rules.

--

### KALI_LAN Interface (Attacker Network)
- **Subnet:** `10.0.2.0/24`
- **Gateway:** `10.0.2.1`
- **Primary host:** `kali-attck`

This interface represents an **untrusted internal segment**.

By default:
- It has no access to internal networks
- It is fully isolated except when temporary rules are enabled for attack simulations

See below screenshot:<br>
[pfsense_interfaces]

---

## Firewall Design Philosophy

The firewall configuration follows a **default-deny** approach:

- All traffic is blocked by default
- Only explicitly required communication is allowed
- Rules are written with minimal scope and clear intent
- Trust is based on **network zone**, not convenience

This approach mirrors real enterprise firewall policies and supports SOC-level visibility.

---

## Management Access Model

Management access is intentionally restricted.

### Allowed Management Actions
Only `admin-ws` is allowed to:
- Access pfSense Web GUI
- SSH into pfSense
- Access Splunk management interfaces
- Administer IDS and SIEM components

### Denied Management Actions
- No other LAN hosts may SSH to servers
- No attacker network hosts may access management interfaces
- No direct management access from untrusted zones

This enforces a clear separation between **administrative** and **non-administrative** traffic.

---

## Inter-LAN Communication Rules

### LAN → SERVER_LAN
Allowed:
- Management traffic from `admin-ws`
- Log forwarding where required
- Access to Splunk dashboards

Denied:
- Unrestricted lateral movement
- Unauthorized SSH access

--

### KALI_LAN → Internal Networks
Denied by default.

Temporary rules may be enabled to:
- Perform controlled attack simulations
- Validate IDS and SIEM detection capabilities

These rules are explicitly documented and disabled after testing.

--

### SERVER_LAN → LAN
Restricted:
- Log responses
- Necessary service replies

No general-purpose access is allowed.

---

## Outbound Internet Access

Outbound access is controlled via NAT:

- Internal networks are translated using pfSense outbound NAT
- No inbound internet access is allowed
- Outbound connectivity is used for:
  - System updates
  - Tool installation
  - Controlled testing

This mirrors a typical enterprise egress model.

---

## Logging and Visibility

pfSense contributes to security visibility by:

- Logging firewall decisions
- Tracking connection states
- Providing network-level context for IDS alerts

These logs are forwarded to Splunk and correlated with:
- Host-based logs
- IDS alerts
- Authentication events

This allows end-to-end visibility from firewall to SIEM.

---

## Security Trade-Offs and Design Decisions

Some design choices were intentional:

- pfSense operates as a **routing firewall**, not an IPS
- IDS is deployed in passive mode for visibility
- Some background noise is expected due to virtualized networking

These trade-offs are documented and handled at the detection and analysis layer rather than by over-restrictive firewall rules.

---

## Summary

The pfSense configuration provides:

- Clear network segmentation
- Controlled management access
- Realistic firewall policy design
- Strong foundation for IDS and SIEM integration

This firewall design supports the overall goal of the lab: demonstrating **defensive security reasoning**, **SOC workflows**, and **practical network security controls** rather than perfect isolation.
