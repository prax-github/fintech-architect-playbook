# Fintech Compliance Interview Playbook  
## File: 01D_IRDAI_Compliance_DeepDive.md

## 🎯 Purpose
What an architect must implement for **IRDAI** compliance in digital insurance platforms: policyholder protection, KYC/AML, data privacy, BCP/DR.

---

## 🧭 Applicability
- Insurers, brokers, web aggregators, TPAs, InsurTechs handling policy issuance, servicing, claims, payments.

---

## 🔑 IRDAI Expectations (Architecture Focus)
1. **Policyholder Data Protection**
   - PII & health data (**sensitive**) → strong encryption, fine-grained access, masking in logs/BI.
   - Consent capture & purpose limitation; consent withdrawal workflows.

2. **KYC/AML**
   - KYC at onboarding and at **material changes**; FATF-aligned AML checks, sanctions/PEP screening.
   - Claims fraud analytics; suspicious activity reporting workflows.

3. **Claims & Customer Service SLAs**
   - TAT tracking for claims/endorsements/refunds; transparent status comms.
   - Evidence attachments (medical, invoices) → secure, access-controlled object stores.

4. **Business Continuity**
   - DR/BCP aligned to criticality; regular drills; vendor DR attestations for outsourced functions.

5. **Distribution & Intermediaries**
   - POSP/Agent onboarding KYC; training/PoS certifications.
   - Web aggregator rules: unbiased listing, disclosure, audit trails of recommendations.

6. **Payments & Premiums**
   - Mandates/eNACH; premium refunds; PCI alignment if cards involved; reconciliation + chargeback flows.

---

## 🏗️ Architecture Patterns for IRDAI
- **Privacy by Design** → Data minimization, attribute-based access (**ABAC**), consent receipts tied to policyholder ID, purpose-bound tokens.
- **Claims** → Case pipelines with SLA timers, **document intelligence** (OCR) over secure buckets, PII-aware redaction.
- **Fraud** → Feature store + rules + ML (graph signals across claimant, hospital, agent).
- **Audit** → **Event sourcing** of policy/claim state; immutable logs; evidence provenance metadata.

---

## 🔐 Security Controls
- **Health/medical** docs → client-side encryption, short-lived pre-signed URLs, private subnets.
- **Key Mgmt** → HSM/KMS, envelope encryption; rotation with dual control.
- **Third-Party** → DPA, data-flow restrictions, **right to audit**, breach notification clauses.

---

## ✅ IRDAI Checklist
- [ ] Consent flows (create/update/revoke) with receipts & timestamps.
- [ ] PII/health data encryption + ABAC + masked telemetry.
- [ ] Claims SLA orchestration with audit & evidence lineage.
- [ ] AML/KYC screens + STR workflows.
- [ ] DR drills & vendor attestations.
- [ ] Premium collection/refunds + reconciliation & chargebacks.

---

## 🧪 Interview Q&A
**Q1. Design secure storage for claims documents.**  
**A**: Private bucket + server-side encryption (KMS) + client-side optional for high-risk docs; pre-signed, short-TTL URLs; VPC endpoints; DLP scans; immutable audit of access.

**Q2. How to implement consent & purpose limitation?**  
**A**: Consent registry service; mint **purpose-bound tokens** that gate access; OPA policies enforce request purpose vs consent scope; revoke path invalidates tokens and future access.

---

## 🧩 Case Prompt
Describe a **claims fraud detection** design using rules + ML, with **explainability** for reviewers and **tamper-evident** audit trails.
