# Fintech Compliance Interview Playbook  
## File: 01C_SEBI_Compliance_DeepDive.md

## 🎯 Purpose
Architectural guardrails for **SEBI** compliance when building trading/investing/wealth tech: KYC/KRA, data retention, surveillance, investor protection.

---

## 🧭 Applicability
- Brokers, DP participants, RTA/RTI, RIAs, PMS/AMC tech, market data vendors, wealth platforms, order/risk management systems (OMS/RMS).

---

## 🔑 Key SEBI Expectations (Architecture Lens)
1. **KYC & KRA Integration**
   - Capture and sync with **KYC Registration Agencies**; maintain accurate, current KYC.
   - Consent receipts, change-tracking, audit of KYC modifications.

2. **Surveillance & Market Abuse Monitoring**
   - Real-time/near-real-time **surveillance rules**: spoofing/layering/pump-dump alerts.
   - Case workflows, escalation, evidence retention.

3. **Data Retention & Integrity**
   - Trade/order logs, call recordings (if applicable), **immutable** storage with **WORM/append-only** semantics.
   - Time-stamped (NTP-synced), hash-chained archives.

4. **Investor Protection**
   - Risk disclosures, suitability checks (for RIAs), advisory vs execution separation.
   - Grievance redressal integration (SCORES), transparent fees.

5. **Business Continuity**
   - DR/BCP with RTO/RPO consistent with exchange obligations; peak hour simulations.

6. **Cybersecurity**
   - VA/PT, Red-team exercises as mandated; vendor risk; SOC monitoring.
   - Strong **segregation of OMS/RMS**, least privilege to production.

---

## 🏗️ Architecture Patterns Mapped to SEBI
- **Surveillance** → Stream processing (Kafka/Flink) → rule engine/ML → alert queues → **case management**.
- **Immutability** → WORM object storage, **event sourcing** for order/trade lifecycle, notarized hash chains.
- **Suitability** → **Policy engine (OPA)** evaluating investor profile vs product risk; step-up consent.
- **Evidence** → Data lineage metadata, signed logs, clock sync, **tamper-evident** archives.

---

## 🔐 Security & Privacy
- **PII** minimization, masking in non-prod, synthetic data.
- **Key Mgmt**: KMS/HSM, rotation, dual control for critical material.
- **Access**: Break-glass with approvals; full session recording for trader tools where required.

---

## ✅ SEBI Checklist
- [ ] KRA integrations with robust retries + idempotency.
- [ ] Surveillance rules + ML signals + case management SLAs.
- [ ] WORM storage for trade/order logs with retention policies.
- [ ] SCORES/GRM integration; customer comms templates.
- [ ] BCP/DR drills; peak load stress tests around market open/close.
- [ ] Red-team/VA-PT findings closed; SOC use-cases tuned.

---

## 🧪 Interview Q&A
**Q1. How to ensure immutable trade logs?**  
**A**: Event-sourced trade lifecycle + WORM object storage; hash chain of daily logs; external timestamping; periodic notarization; limited privileged paths with approvals.

**Q2. Build a basic surveillance pipeline.**  
**A**: Market/order streams → normalization → rules (price/volume anomaly, wash trades) + ML scoring → alert priority → **case queue** with linkage to trades/accounts → audit-ready resolution.

---

## 🧩 Case Prompt
Explain how you’d layer **surveillance** over an existing broker OMS without rewriting it, using **ACL + stream taps** and a **separate case system**.
