# Fintech Compliance Interview Playbook
## File: 02C_GDPR_and_CCPA_Privacy_DeepDive.md

## 🎯 Purpose
Engineering playbook for **GDPR** (EU) and **CCPA/CPRA** (California) obligations: data minimization, subject rights, consent, and privacy by design.

---

## 🔑 Core Principles → Design Tactics
1. **Data Minimization & Purpose Limitation**
   - Collect only necessary attributes; annotate purpose in schema; enforce at read time (OPA policies).

2. **Lawful Basis & Consent**
   - Consent registry with **purpose-bound tokens**; explicit/withdrawable; granular scopes; per-purpose TTLs.

3. **Data Subject Rights**
   - **Access (DSAR)**, **Rectification**, **Erasure (RTBF)**, **Restriction**, **Portability**, **Objection**.
   - Build **Rights Service** to orchestrate identity proofing, data discovery, redaction, rendering, and deletion workflows.

4. **Data Residency & Transfers**
   - Geo-pinned storage; SCCs/DTAs if cross-border; pseudonymize where possible.

5. **Security & Breach**
   - “Appropriate measures”: encryption, access control, audit logging; breach detection & 72-hr notification runbook (GDPR).

6. **Do Not Sell / Sharing Restrictions (CCPA/CPRA)**
   - Flags in user profile; enforcement at ad/analytics pipelines; do-not-track propagation.

---

## 🏗️ Architecture Building Blocks
- **PII Catalog** with lineage & purpose tags; data discovery scanners.
- **Consent Service** (create/update/revoke), emits policy changes to PDP.
- **Privacy Gateway** that evaluates request purpose vs consent & residency.
- **Rights Orchestrator**: DSAR/RTBF with dependency graph & tombstoning.
- **Pseudonymization/Tokenization** libs for analytics & test data.
- **Privacy Telemetry**: access events, consent changes, DSAR SLAs.

---

## ✅ Engineer’s Checklist
- [ ] Purpose tags in schemas; PDP enforces purpose at API layer.
- [ ] DSAR/RTBF automated end-to-end with proofs & audit.
- [ ] Geo-pinning & transfer controls; contracts/SCCs documented.
- [ ] Data maps updated; discovery scans run on schedule.
- [ ] Consent UIs and fine-grained toggles; revocation propagation tested.
- [ ] “Do Not Sell” honored in ads/attribution/ML pipelines.

---

## 🧪 Interview Q&A
**Q1. Implement GDPR RTBF at scale.**  
**A**: Tombstone ID → orchestrate deletes across services + backups where feasible; retain legal-required records separately, pseudonymized; produce artifacts (deletion logs, hashes).

**Q2. Balance analytics with minimization.**  
**A**: Use pseudonymized IDs, differential privacy where apt, aggregated metrics by default, strict access controls; raw PII only in restricted zones.

**Q3. Enforce purpose limitation in microservices.**  
**A**: Central PDP (OPA) with purpose in JWT/headers; services check policy on each access; audit decision & purpose.

---

## 🧩 Design Prompt
Design a **DSAR platform** that discovers PII across polyglot stores, assembles a report, and executes RTBF with audit evidence.
