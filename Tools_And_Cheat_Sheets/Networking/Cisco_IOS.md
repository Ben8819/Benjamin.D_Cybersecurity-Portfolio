# Network Command Cheat Sheet

> Cisco IOS — Switch & Router  

---

## Table of contents

- [VLAN](#vlan)

---

## VLAN

### Create & name VLANs

| Command        | Mode   | Description                                               |
|----------------|--------|-----------------------------------------------------------|
| `vlan <id>`    | Global | Create a VLAN and enter VLAN config mode (e.g. `vlan 10`) |
| `name <name>`  | VLAN   | Assign a human-readable name to the VLAN                  | 
| `no vlan <id>` | Global | Delete a VLAN from the database                           |

---

### Access ports (untagged)

| Command                       | Mode      | Description                                          |
|-------------------------------|-----------|------------------------------------------------------|
| `interface <port>`            | Global    | Enter interface config mode (e.g. `interface fa0/1`) |
| `switchport mode access`      | Interface | Force port into access (untagged) mode               |
| `switchport access vlan <id>` | Interface | Assign port to a specific VLAN                       |
| `switchport nonegotiate`      | Interface | Disable DTP negotiation on the port                  |

> **Best practice:** Always explicitly set `switchport mode access` — don't rely on auto-negotiation.  
> Assign unused ports to a dead VLAN (e.g. `vlan 999`) and shut them down.

---

### Trunk ports (tagged / 802.1Q)

| Command                                     | Mode      | Description                                                          |
|---------------------------------------------|-----------|----------------------------------------------------------------------|
| `switchport mode trunk`                     | Interface | Force port into trunk (tagged) mode                                  |
| `switchport trunk encapsulation dot1q`      | Interface | Set 802.1Q encapsulation (required on older switches)                |
| `switchport trunk allowed vlan <list>`      | Interface | Restrict allowed VLANs on the trunk (e.g. `10,20,30`)                |
| `switchport trunk allowed vlan add <id>`    | Interface | Add a VLAN to the existing allowed list                              |
| `switchport trunk allowed vlan remove <id>` | Interface | Remove a VLAN from the allowed list                                  |
| `switchport trunk native vlan <id>`         | Interface | Set native VLAN (untagged traffic on trunk) — must match both ends   |

> **Best practice:** Prune VLANs you don't need on each trunk — don't allow all by default.  
> Native VLAN mismatch causes a CDP warning and possible traffic leaking.

---

### Router-on-a-stick (inter-VLAN routing)

| Command                    | Mode          | Description                                                      |
|----------------------------|---------------|------------------------------------------------------------------|
| `interface <iface>.<id>`   | Global        | Create a sub-interface for a VLAN (e.g. `interface g0/0.10`)     |
| `encapsulation dot1q <id>` | Sub-interface | Tag the sub-interface to a VLAN. Append `native` for native VLAN |
| `ip address <ip> <mask>`   | Sub-interface | Assign the gateway IP for that VLAN's subnet                     |
| `no shutdown`              | Interface     | Bring the physical interface up                                  |

> The physical interface must have `no shutdown` and **no IP address** assigned directly.  
> The switch port connecting to the router must be in trunk mode and allow all relevant VLANs.

---

### SVI — Layer 3 switch inter-VLAN routing

| Command                  | Mode   | Description                                          |
|--------------------------|--------|------------------------------------------------------|
| `ip routing`             | Global | Enable L3 routing on the switch (required first)     |
| `interface vlan <id>`    | Global | Create a Switch Virtual Interface (SVI) for the VLAN |
| `ip address <ip> <mask>` | SVI    | Set the gateway IP for that VLAN                     |
| `no shutdown`            | SVI    | Bring the SVI up                                     |

---

### Verification

| Command                             | Description                                                        |
|-------------------------------------|--------------------------------------------------------------------|
| `show vlan brief`                   | List all VLANs, names, status, and assigned ports                  |
| `show vlan id <id>`                 | Detailed info for a specific VLAN                                  |
| `show interfaces trunk`             | List trunk ports, encapsulation, allowed VLANs, and native VLAN    |
| `show interfaces <port> switchport` | Mode, access VLAN, trunk VLANs, and DTP status for a port.         |
| `show running-config`               | Full config — search for `vlan`, `switchport`, or `interface vlan` |
| `show ip interface brief`           | Verify SVI and sub-interface IP addresses and states               |

---

### Troubleshooting

| Symptom                  | Likely cause & fix                                                                                         |
|--------------------------|------------------------------------------------------------------------------------------------------------|
| VLAN not in database     | Port assigned to a VLAN that hasn't been created → create it first with `vlan <id>`                        |
| Native VLAN mismatch     | CDP warning — both ends of the trunk must have the same native VLAN                                        |
| VLAN not in allowed list | Traffic silently dropped → check `show interfaces trunk` allowed column                                    |
| SVI is down/down         | VLAN must exist in the database AND have at least one active access port assigned                          |
| No inter-VLAN routing    | L3 switch: verify `ip routing` is enabled. Router-on-a-stick: check sub-interfaces and trunk allowed VLANs |

---

*More sections coming soons...*
