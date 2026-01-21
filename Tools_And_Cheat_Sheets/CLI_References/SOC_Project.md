# Common Commands Used in the SOC Home Lab Project

This document lists commonly used command-line tools and commands executed during the [SOC Home Lab project/](Projects/SOC_Home_Lab)<br>
It is intended to demonstrate hands-on familiarity with Linux, networking, IDS tooling, and investigation workflows.

---

## 1. System & Environment Setup

`hostnamectl`<br>
`uname -a`<br>
`lsb_release -a`<br>
`ip a`<br>
`ip r`<br>
`df -h`<br>
`free -h`

Used to validate system identity, OS version, network configuration, and resource availability.

---

## 2. Package Installation & Updates

`apt update`<br>
`apt upgrade`<br>
`apt install suricata`<br>
`apt install splunkforwarder`<br>
`apt install tcpdump`<br>
`apt install curl`<br>
`apt install net-tools`

Used to install and maintain core security and troubleshooting tools.

---

## 3. Service Management (systemd)

`systemctl status suricata`<br>
`systemctl start suricata`<br>
`systemctl restart suricata`<br>
`systemctl enable suricata`<br>

`systemctl status splunk`<br>
`systemctl start splunk`<br>
`systemctl restart splunk`

Used to control IDS and SIEM services and verify operational status.

---

## 4. Log Management & Visibility

`ls -lah /var/log/suricata/`<br>
`tail -f /var/log/suricata/eve.json`<br>
`tail -f /var/log/syslog`<br>
`journalctl -u suricata`<br>
`journalctl -xe`

Used to validate log generation, ingestion readiness, and troubleshoot issues.

---

## 5. Network Traffic Capture & Analysis

`ip link`<br>
`ip link set eth1 promisc on`<br>
`tcpdump -i eth1`<br>
`tcpdump -i eth1 -nn -c 100`

Used to confirm mirrored/SPAN traffic visibility and packet-level inspection.

---

## 6. Suricata Rules & Detection Validation

`suricata -T -c /etc/suricata/suricata.yaml`<br>
`suricata-update`<br>
`suricata-update list-sources`

Used to validate configuration, update rule sets, and ensure detection capability.

---

## 7. Splunk Configuration & Verification

`/opt/splunk/bin/splunk status`<br>
`/opt/splunk/bin/splunk start`<br>
`/opt/splunk/bin/splunk restart`<br>
`/opt/splunk/bin/splunk list forward-server`

Used to manage Splunk services and confirm forwarder / indexer connectivity.

---

## 8. File & Permission Management

`ls -l`<br>
`chmod 644 /var/log/suricata/eve.json`<br>
`chown splunk:splunk /var/log/suricata/eve.json`

Used to ensure proper access rights for log ingestion.

---

## 9. Connectivity & Traffic Generation (Attack Simulation)

`ping`<br>
`curl http://target`<br>
`curl -I http://target`<br>
`nmap -sS -Pn target`<br>
`nmap -p 80,443 target`

Used to generate detectable network activity and validate alerting.

---

## 10. Investigation & Validation

`grep "alert" /var/log/suricata/eve.json`<br>
`jq '.alert.signature' /var/log/suricata/eve.json`<br>
`ps aux | grep suricata`<br>
`ss -tulpn`

Used to investigate alerts, validate detections, and confirm service behavior.

---

## 11. System Troubleshooting

`dmesg`<br>
`top`<br>
`htop`<br>
`vmstat`

Used to diagnose performance or stability issues during lab operation.- Commands listed here were executed in a controlled lab environment

---

12. Safe Administrative Practices

`sudo -l`<br>
`sudoedit /etc/suricata/suricata.yaml`<br>
`history`

Used to track changes and safely modify configuration files.

---
