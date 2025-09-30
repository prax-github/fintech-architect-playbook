# Fintech Compliance Interview Playbook  
## File: 04_Auditability_and_Observability.md  

---

## 🎯 Purpose  
This file covers **auditability and observability** practices for fintech systems.  
It ensures compliance with RBI, PCI-DSS, SOX, SEBI, GDPR, and other frameworks that mandate **immutable logs, monitoring, and evidence generation**.  

---

## 📝 Audit Logging Patterns  

### Immutable, Append-Only Logs  
- **Regulatory Need**: SOX, SEBI, RBI require tamper-proof audit trails.  
- **Implementation**:  
  - Append-only event store (Kafka, Event Sourcing).  
  - WORM (Write Once, Read Many) storage for archives.  
  - Hash-chained logs for integrity (Merkle trees).  
- **Data Captured**:  
  - Who (user/service ID).  
  - What (action, resource).  
  - When (timestamp, synchronized NTP).  
  - Where (IP/device, region).  
  - Why (request context, correlation ID).  

### Redaction & Masking  
- Never log PAN, CVV, Aadhaar, SSN in plain text.  
- Mask or tokenize sensitive fields before audit persistence.  

**Architecture Patterns**:  
- Centralized Audit Service (write-only).  
- Dual-plane storage: hot (queryable), cold (immutable).  
- Signed audit entries → periodically notarized.  

---

## 🔍 Distributed Tracing & Log Aggregation  

### Distributed Tracing  
- **Need**: To trace transaction lifecycle across microservices (esp. UPI, BBPS, card payments).  
- **Implementation**:  
  - Correlation IDs (RRN, STAN, transaction ID) propagated across services.  
  - OpenTelemetry or Jaeger for spans/traces.  
  - Mask sensitive fields in trace payloads.  

### Centralized Logging  
- Log aggregation into ELK/EFK, Splunk, or cloud-native observability stack.  
- Fine-grained RBAC for log access (auditors vs engineers).  
- Immutable storage with retention policies (5–10 years as per regulation).  

---

## 📊 Compliance Metrics  

- **Customer-Centric Metrics**:  
  - SLA breaches (e.g., BBPS complaint TAT).  
  - Average wait time (call centers, disputes).  
  - Agent utilization (if applicable).  

- **Security & Risk Metrics**:  
  - Unauthorized access attempts.  
  - Fraud alerts generated/closed.  
  - Key rotations & privileged access events.  

- **System Health Metrics**:  
  - Latency, error rates, uptime (per regulatory SLOs).  
  - Settlement failures, reconciliation breaks.  
  - BCP/DR drill success rate.  

**Architecture Patterns**:  
- Prometheus/Grafana dashboards for real-time compliance SLAs.  
- Alerting on thresholds → SIEM/SOAR integration.  

---

## 📑 Reporting for Audits  

### Regulatory Reports  
- **RBI/NPCI**: Settlement + reconciliation reports, complaint resolution dashboards.  
- **SEBI**: Trade/order logs, surveillance alerts, SCORES grievance closure reports.  
- **SOX/FFIEC**: Change management, access reviews, incident logs.  
- **AML**: STR/CTR reports for FIU-IND, FinCEN.  

### Evidence Pipelines  
- Automate **evidence collection**:  
  - Access reviews from IAM.  
  - Change approvals from CI/CD.  
  - Security scans (SAST, DAST, PT).  
  - DR drill results.  
- Store in **Compliance Data Lake** → Generate auditor-ready reports.  

---

## ✅ Checklist for Auditability & Observability  

- [ ] Immutable, append-only logs for financial transactions.  
- [ ] Centralized logging with masking for PII/CHD.  
- [ ] Distributed tracing with correlation IDs (RRN, STAN).  
- [ ] Compliance metrics dashboards (SLA, fraud, DR, security).  
- [ ] Evidence pipelines for audits (change, access, scans, DR).  
- [ ] WORM/archival storage with retention policies.  

---

## 🧪 Interview Q&A  

**Q1. How do you ensure audit trails are tamper-proof?**  
**A**: Append-only event logs + WORM storage; hash chains for integrity; external timestamping; restricted access; auditor dashboards with evidence artifacts.  

**Q2. How do you add tracing to a payments system (e.g., UPI)?**  
**A**: Use RRN as correlation ID across microservices; propagate in headers; OpenTelemetry spans; masked fields; queryable in Jaeger/Splunk for root-cause + compliance tracing.  

**Q3. How do you handle GDPR with audit logs (since logs store personal data)?**  
**A**: Minimize data in logs (IDs/tokens instead of raw PII); apply masking; enforce retention policies; DSAR workflows purge/minimize logs where feasible.  

**Q4. What metrics do regulators expect?**  
**A**: SLA breaches (complaints, claims), settlement success, uptime, fraud alerts closed, DR drill evidence — all mapped to dashboards for ongoing compliance.  

---

## 📚 References  

- [OpenTelemetry](https://opentelemetry.io/)  
- [RBI Cybersecurity Guidelines](https://rbi.org.in/)  
- [SEBI IT/Cybersecurity Circulars](https://www.sebi.gov.in/)  
- [PCI-DSS Logging Requirements](https://www.pcisecuritystandards.org/)  
- [SOX Audit Controls](https://www.sec.gov/spotlight/sarbanes-oxley.htm)  

---
