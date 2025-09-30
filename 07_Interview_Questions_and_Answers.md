# Fintech Compliance Interview Playbook  
## File: 07_Interview_Questions_and_Answers.md  

---

## 🎯 Purpose  
This file provides **interview-ready compliance Q&A** for fintech architects.  
Each answer follows a **Problem → Approach → Trade-offs → Patterns** structure to demonstrate depth.  

---

## 🔑 Common Compliance-Focused Questions & Answers  

---

### Q1. How would you design a **PCI-DSS compliant payment system**?  

**Problem**  
- Handle cardholder data securely.  
- Reduce audit scope while maintaining performance.  

**Approach**  
- Tokenize PAN at ingress → downstream services use tokens only.  
- Isolate **Cardholder Data Environment (CDE)** in dedicated network segment.  
- Enforce AES-256 encryption at rest, TLS 1.3 in transit.  
- HSM/KMS custody with dual control for keys.  

**Trade-offs**  
- Vault adds latency → mitigate with in-memory token cache.  
- Segmentation increases infra complexity → justified for compliance.  

**Patterns**  
- **Tokenization Service (HSM-backed)**.  
- **ACL** to isolate PCI flows from core services.  
- **Zero Trust + mTLS** for service-to-service calls.  

---

### Q2. How do you ensure **RBI Data Localization compliance** in cloud-native systems?  

**Problem**  
- Payments data must remain in India.  
- Still need high availability + DR.  

**Approach**  
- Pin all DBs, buckets, and backups to India regions.  
- Dual-site active-active setup across Indian seismic zones.  
- DLP rules + egress firewalls to prevent cross-border leaks.  

**Trade-offs**  
- Can’t use multi-region (global) DR.  
- Solution: India-only dual region, plus pseudonymized replicas abroad for analytics.  

**Patterns**  
- **Region-aware storage + geo-fenced VPCs**.  
- **Immutable audit logs** for proof.  
- **Compliance Evidence Pipeline** to show RBI auditors configs & DR drills.  

---

### Q3. How do you design a system for **GDPR Right-to-Erasure (RTBF)**?  

**Problem**  
- Users can request deletion of personal data.  
- Conflict with financial data retention laws (SOX, SEBI).  

**Approach**  
- Build **DSAR service**: discovery, report, purge.  
- Tombstone IDs across services → pseudonymize where retention required.  
- Segregate audit logs from live data.  

**Trade-offs**  
- Pure deletion may break audit trails.  
- Solution: pseudonymized storage with retained audit entries.  

**Patterns**  
- **Event Sourcing** with delete events (tombstones).  
- **ABAC policies** with purpose/consent tags.  
- **Immutable audit logs** separated from PII.  

---

### Q4. How do you balance **latency with audit logging requirements**?  

**Problem**  
- Audit logs must be complete, immutable, and regulatory-grade.  
- But they shouldn’t impact transaction performance.  

**Approach**  
- Use **async logging pipeline** (Kafka → Audit Store).  
- Log correlation IDs instead of full payloads.  
- Mask sensitive fields before persistence.  

**Trade-offs**  
- Async may risk partial log loss → mitigate with retries, DLQs, quorum writes.  

**Patterns**  
- **CQRS**: separate write (txn) vs read (audit).  
- **Immutable WORM storage** for compliance archives.  
- **Distributed tracing** for correlation.  

---

### Q5. How do you implement **AML transaction monitoring**?  

**Problem**  
- Need to detect suspicious transactions (large, unusual, sanctioned).  
- Must support regulatory reporting (STR/CTR).  

**Approach**  
- Kafka ingestion → rules engine + ML anomaly detection.  
- Alerts into AML case management system.  
- Generate CTR/STR automatically for regulator.  

**Trade-offs**  
- Too many alerts (false positives) overwhelm analysts.  
- Solution: ML-based risk scoring + prioritization.  

**Patterns**  
- **Graph DB** for suspicious networks.  
- **CQRS query models** for AML dashboards.  
- **Immutable logs** for auditor review.  

---

### Q6. How do you design **KYC workflows** for fintech onboarding?  

**Problem**  
- Regulators (RBI, SEBI, IRDAI, FATF) mandate KYC for customer onboarding.  

**Approach**  
- Orchestrate Aadhaar/PAN/CKYC integrations (India) or OCR + face match (Global).  
- Build consent-driven onboarding flows.  
- AML screening during onboarding.  

**Trade-offs**  
- Video KYC improves compliance but adds friction.  
- Tradeoff UX vs compliance → segment flows (low vs high value customers).  

**Patterns**  
- **KYC Service** with async doc verification.  
- **Consent Registry** tied to customer ID.  
- **Encrypted Document Vault** with pre-signed URLs.  

---

### Q7. How do you ensure **audit trails under SOX/SEBI**?  

**Problem**  
- Regulators need immutable financial reporting and trade logs.  

**Approach**  
- Event-sourced ledgers with double-entry accounting.  
- Append-only logs, WORM archives, hash chaining.  
- Periodic reconciliation + evidence dashboards.  

**Trade-offs**  
- High storage overhead → optimize with cold archival tiers.  

**Patterns**  
- **Event Sourcing + CQRS** for replay + reports.  
- **Immutable WORM storage**.  
- **Evidence pipeline** → auto-collect change/access artifacts.  

---

## ✅ Interview-Ready Tips  

- Always **link compliance to architecture patterns**.  
- Show **trade-offs** (latency vs auditability, UX vs KYC friction, DR vs data localization).  
- Use **storytelling from case studies** (BBPS, digital assets, AML pipelines).  
- Use phrases like:  
  - “Regulators expect **immutable auditability**, so we used Event Sourcing + WORM.”  
  - “To reduce PCI scope, we tokenized PAN early and isolated the CDE.”  
  - “RBI localization forced us to design dual-India-region DR.”  

---

## 📚 References  

- [PCI-DSS Requirements](https://www.pcisecuritystandards.org/)  
- [RBI/NPCI Compliance](https://rbi.org.in/) | [https://www.npci.org.in/](https://www.npci.org.in/)  
- [GDPR Principles](https://gdpr-info.eu/)  
- [SOX Audit Controls](https://www.sec.gov/spotlight/sarbanes-oxley.htm)  
- [FATF AML Guidelines](https://www.fatf-gafi.org/)  

---
