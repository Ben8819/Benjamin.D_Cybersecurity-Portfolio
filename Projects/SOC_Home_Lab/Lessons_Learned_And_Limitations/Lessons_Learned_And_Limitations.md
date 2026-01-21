## Lessons Learned & Limitations

This project intentionally reflects a **realistic learning environment**, including technical challenges, constraints, and trade-offs.  
Documenting these aspects is essential to demonstrate professional maturity and transparency.

### Lessons Learned

- **Alert noise is unavoidable** in IDS deployments  
  Raw IDS output is often dominated by benign protocol-level events. Effective security monitoring depends on classification, filtering, and analyst judgment.

- **Signal quality matters more than alert volume**  
  A dashboard showing no suspicious alerts can indicate a healthy environment and well-tuned detections.

- **Correlation is essential**  
  Individual alerts are rarely conclusive. Confidence comes from correlating:
  - Network alerts
  - Authentication failures
  - Firewall logs
  - Host behavior

- **Dashboards must support analyst workflows**  
  Visualizations are only useful if they answer real investigative questions quickly.

- **Lab instability is realistic**  
  Network issues, tooling limitations, and partial executions mirror real-world operational friction.

---

### Limitations

- Limited attack diversity due to lab scope
- No real user traffic or production workloads
- No SOAR or automated response mechanisms
- No external threat intelligence enrichment
- Single analyst environment (no ticketing or escalation workflows)

These limitations are acknowledged and intentionally documented.

---

## Future Improvements

Several enhancements could be implemented to extend this project:

- Add additional attack simulations:
  - Web exploitation
  - Lateral movement
  - Privilege escalation
- Introduce severity-based alert escalation
- Add automated alert summaries
- Implement SOAR-style response playbooks
- Enrich events with:
  - GeoIP data
  - Threat intelligence feeds
- Expand dashboards for:
  - Firewall rule analysis
  - East-west traffic visibility
- Add alert thresholds and notifications

These improvements reflect realistic SOC evolution paths.

---

## Skills Demonstrated

This project demonstrates practical skills across multiple domains:

### Security Monitoring & Detection
- IDS deployment and tuning (Suricata)
- Alert classification and noise reduction
- Detection validation through attack simulation

### SIEM & Log Management
- Splunk installation and configuration
- Multi-source log ingestion
- Index and sourcetype management
- Dashboard creation and refinement

### Networking & Firewalling
- pfSense deployment and rule design
- Network segmentation and access control
- Understanding of traffic flow and visibility

### SOC Operations
- Alert triage and investigation workflows
- Cross-source correlation
- Timeline reconstruction
- Incident impact assessment

### Engineering & Problem Solving
- Troubleshooting complex lab issues
- Iterative refinement of detections
- Documentation-driven development

---

## Why This Project Is SOC-Relevant

This lab was designed to reflect **real SOC monitoring conditions**, not idealized demo environments.

Key SOC-aligned characteristics:

- Imperfect data and background noise
- Analyst-driven interpretation
- Focus on behavior, not single alerts
- Emphasis on investigation workflows
- Transparent handling of limitations

Rather than showcasing isolated tools, this project demonstrates the ability to:
- Design a monitoring architecture
- Detect and analyze suspicious activity
- Reduce false positives
- Communicate findings clearly

These are **core competencies expected of an entry-level SOC analyst**.

---

## Final Notes

This project represents a complete end-to-end security monitoring pipeline:

- From network traffic
- To IDS detection
- To centralized SIEM analysis
- To analyst-driven investigation

It serves as a practical foundation for further specialization in:
- SOC operations
- Detection engineering
- Incident response
