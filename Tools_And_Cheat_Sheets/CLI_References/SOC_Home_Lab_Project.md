# Common Commands Used in the SOC Home Lab Project

This document lists commonly used command-line tools and commands executed during the [SOC Home Lab](/Projects/SOC_Home_Lab) project.<br>
It is intended to demonstrate hands-on familiarity with Linux, networking, IDS tooling, and investigation workflows.

---

## 1 - System & Environment Setup

```bash
hostnamectl
uname -a
lsb_release -a
ip a
ip r
df -h
free -h
```

Used to validate system identity, OS version, network configuration, and resource availability.

---

## 2 - Package Installation & Updates

```bash
apt update
apt upgrade
apt install suricata
apt install splunkforwarder
apt install tcpdump
apt install curl
apt install net-tools
```

Used to install and maintain core security and troubleshooting tools.

---

## 3 - Service Management (systemd)

```bash
systemctl status suricata
systemctl start suricata
systemctl restart suricata
systemctl enable suricata

systemctl status splunk
systemctl start splunk
systemctl restart splunk
```

Used to control IDS and SIEM services and verify operational status.

---

## 4 - Log Management & Visibility

```bash
ls -lah /var/log/suricata/
tail -f /var/log/suricata/eve.json
tail -f /var/log/syslog
journalctl -u suricata
journalctl -xe
```

Used to validate log generation, ingestion readiness, and troubleshoot issues.

---

## 5 - Network Traffic Capture & Analysis

```bash
ip link
ip link set eth1 promisc on
tcpdump -i eth1
tcpdump -i eth1 -nn -c 100
```

Used to confirm mirrored/SPAN traffic visibility and packet-level inspection.

---

## 6 - Suricata Rules & Detection Validation

```bash
suricata -T -c /etc/suricata/suricata.yaml
suricata-update
suricata-update list-sources
```

Used to validate configuration, update rule sets, and ensure detection capability.

---

## 7 - Splunk Configuration & Verification

```bash
/opt/splunk/bin/splunk status
/opt/splunk/bin/splunk start
/opt/splunk/bin/splunk restart
/opt/splunk/bin/splunk list forward-server
```

Used to manage Splunk services and confirm forwarder / indexer connectivity.

---

## 8 - File & Permission Management

```bash
ls -l
chmod 644 /var/log/suricata/eve.json
chown splunk:splunk /var/log/suricata/eve.json
```

Used to ensure proper access rights for log ingestion.

---

## 9 - Connectivity & Traffic Generation (Attack Simulation)

```bash
ping
curl http://target
curl -I http://target
nmap -sS -Pn target
nmap -p 80,443 target
```

Used to generate detectable network activity and validate alerting.

---

## 10 - Investigation & Validation

```bash
grep "alert" /var/log/suricata/eve.json
jq '.alert.signature' /var/log/suricata/eve.json
ps aux | grep suricata
ss -tulpn
```

Used to investigate alerts, validate detections, and confirm service behavior.

---

## 11 - System Troubleshooting

```bash
dmesg
top
htop
vmstat
```

Used to diagnose performance or stability issues during lab operation.- Commands listed here were executed in a controlled lab environment

---

## 12 - Safe Administrative Practices

```bash
sudo -l
sudoedit /etc/suricata/suricata.yaml
history
```

Used to track changes and safely modify configuration files.

---
