# Fintech Compliance Interview Playbook  
## File: 01A_RBI_Compliance_DeepDive.md

## 🎯 Purpose
A practical, interview-ready deep dive into RBI compliance expectations for fintech/payments platforms, with architecture implications, checklists, and common Q&As.

---

## 📌 Scope & Applicability
- **Entities**: Banks, PPI issuers, NBFCs, PSPs, payment aggregators/gateways, card networks’ India entities, lenders/LLPs integrating with banking rails.
- **Systems**: Core banking & LOS/LMS, payment processing (cards/UPI/IMPS/NEFT), wallets/PPI, reconciliation & settlement, reporting & regulatory interfaces.

---

## 🧭 Core RBI Expectations (Architecture-Relevant)
1. **Data Localization (Payments)**
   - Store **full end-to-end payments data** (as mandated) **within India**.
   - If mirroring/summarization is needed outside India, ensure **only permitted fields** and **reversible identifiers** are absent; apply **irreversible tokenization** where feasible.
   - **Design**: Region-pinned storage (India), geo-fenced backups, regional KMS/HSM.

2. **Information Security Baselines**
   - **Defense-in-depth**: Zero Trust, network micro-segmentation, WAF, DDoS protection, EDR, SIEM/SOAR.
   - **Crypto**: TLS 1.2/1.3, AES-256 at rest, **HSM-backed** key custody for sensitive/payment keys.
   - **Identity**: MFA for ops consoles, strict RBAC/ABAC, JIT privileged access; session hardening.

3. **Risk Management & Governance**
   - Independent **IS audit**, **VA/PT** cycles, third-party risk assessments.
   - **Change management** and **production access control** — approvals + full audit trails.

4. **Customer Protection**
   - **Dispute/chargeback** timelines, TAT commitments, complaint management systems (CMS).
   - Transparent disclosures, consent logging, **opt-out** experiences, grievance redressal dashboards.

5. **Business Continuity & DR**
   - **Dual-site** active-active/active-passive across **separate seismic zones**.
   - **RTO/RPO** targets, quarterly DR drills; switch-over runbooks; immutable backup sets.

6. **Outsourcing/Third-Party**
   - SLAs, data-processing addendums, **right to audit**, exit plans, **security posture attestation**.

---

## 🔐 Data & Security Controls (Minimums)
- **Data classes**: Public, Internal, Confidential, **Sensitive financial**; annotate in schemas.
- **Tokenization**: PAN/card tokens; surrogate keys for payment identifiers.
- **Secrets**: Vaulted (KMS+HSM), rotation policies, dual control for master keys.
- **Observability**: Centralized logs (7–10 yrs retention where applicable), time-sync, tamper-evident storage.

---

## 🏗️ Architecture Patterns Mapped to RBI
- **Data Localization** → Region-pinned storage, S3-like buckets in India, India-region RDS/Kafka.
- **Auditability** → **Event Sourcing + CQRS** for financial events; **immutable append-only** audit logs.
- **Segregation of Duties** → Multi-tenant isolation, separate admin plane, break-glass access with approvals.
- **Legacy integration** → **Anti-Corruption Layer (ACL)** to insulate RBI-driven constraints from internal models.

---

## 📈 Non-Functional Requirements (NFR) Tie-ins
- **Availability**: N+1 infra, AZ-spanning clusters, graceful degradation.
- **Performance**: Crypto offload via HSM, caching (NON-sensitive), back-pressure + rate limits.
- **Compliance**: Evidence pipelines (audit artifacts, policy as code, IaC drift detection).

---

## ✅ Go-Live / Audit Checklist
- [ ] India-region storage enforced (prod + backups).
- [ ] Key custody: HSM + rotation + dual control.
- [ ] DR drill passed within target RTO/RPO.
- [ ] VA/PT + IS audit reports closed.
- [ ] SIEM rules for fraud, PII access, privilege escalations.
- [ ] Outsourcing/TPRMs signed with audit rights.
- [ ] Incident response runbooks & 24×7 escalation paths.

---

## 🧪 Interview Q&A (Concise)
**Q1. Design a payments platform compliant with RBI data-localization.**  
**A**: India-region-only primary + backup stores; cross-border only derived/aggregated non-reversible data; India-region KMS/HSM; data-flow DLP at egress; policy enforcement via OPA at gateways; evidence via config compliance scans.

**Q2. How do you balance DR with localization?**  
**A**: Two Indian regions (separate seismic zones), encrypted, frequent point-in-time recovery; cross-region replication confined to India; immutable backups + tabletop and live failover drills.

---

## 🧩 Case Prompt (Tie to Your BBPS Work)
Describe how you handled **RBI/NPCI audits**, DR drills, and **evidence generation** (change tickets, VA/PT, CMS TAT dashboards).
