# Fintech Compliance Interview Playbook  
## File: 01B_NPCI_Compliance_DeepDive.md

## 🎯 Purpose
Deep dive into **NPCI** compliance across UPI, BBPS, IMPS, RuPay/NFS — with certification flow, controls, and system design patterns.

---

## 🗺️ Roles & Surfaces
- **UPI**: Bank PSP, TPAP, payment gateway, issuer/acquirer, switch connectivity.
- **BBPS**: BBPOU (BOU), Biller/Agent institutions, complaint & reconciliation rails.
- **IMPS**: Member banks/participants, ISO-8583 or equivalent message interfaces.
- **RuPay/NFS**: Card issuance/acquiring, switching, HSM PIN/keys, PCI-DSS alignment.

---

## 🔩 Cross-Cutting NPCI Controls
1. **Crypto & Key Mgmt**
   - **End-to-end payload signing/encryption** as per NPCI specs; key rotation calendars.
   - HSM custody for **PIN/keys** (RuPay/NFS); secure key ceremony procedures.
2. **Channel Security**
   - **Mutual TLS**, replay protection (nonce, timestamp), strict time windows.
   - **Idempotency** keys; duplicate detection; rate limiting & throttling.
3. **Operational**
   - **Certification** (pre-cert, regression, production clearance); periodic re-cert.
   - **Monitoring**: Availability, latency SLOs, T+1/T+0 settlements, reconciliation diffs.
4. **Logging/Audit**
   - Correlation IDs (RRN/Stan), request/response hashes, immutable archives, field-level masking.

---

## 🧭 UPI: Compliance & Architecture
- **SCA & Device Binding**: Bind VPA/app to device; challenge flows; secure in-app storage (keystore).
- **Mandates/Autopay**: Create/modify/cancel with signed requests, expiry, revocation audit.
- **Risk & Fraud**: Velocity rules (payer/payee), geo/device fingerprints, beneficiary risk scoring, step-up auth.
- **Resilience**: Circuit breakers to issuer/PSP endpoints; async retriable queues with strict idempotency; fallback rails.
- **Data**: No sensitive data stored beyond policy; tokenized handles; masked logs.

**Design Tips**
- **Message Adapter Layer** for NPCI specs → internal canonical events.
- **Outbox/Saga** for exactly-once effect across wallet/bank updates.
- **Observability** with span tags: VPA, issuer bank BIN (masked), RRN.

---

## 🧭 BBPS (Bharat BillPay): Compliance & Architecture
- **BOU Responsibilities** (your experience): Onboard billers, validate categories, ensure complaint & reversal SLAs.
- **Encryption**: Channel-level + payload-level per BBPS specs.
- **Reconciliation & Settlement**: **T+1** consistency, **auto-recon jobs**, break reports, maker-checker adjustments.
- **Complaint Mgmt**: Case lifecycle, SLA timers, evidence attachments, escalation matrix.

**Design Tips**
- **Queue-backed complaint workflows** (SLA timers).
- **Reconciliation microservice** with **CQRS read models** for break dashboards.
- **ACL** to shield internal domain from BBPS field/set changes.

---

## 🧭 IMPS: Compliance & Architecture
- **Messaging**: ISO-8583 (or NPCI-defined format) with MACs, field validations.
- **Idempotency**: Use **RRN + STAN**; de-dupe store; duplicate window policy.
- **Cut-off/Settlement**: Handle holiday/NEFT/IMPS combinations; **retry with backoff**; poison queues to manual repair.

---

## 🧭 RuPay/NFS: Compliance & Architecture
- **PCI-DSS Alignment**: Cardholder data environment (CDE) segmentation; PAN masking/tokenization; **no CVV storage**.
- **PIN Security**: **HSM** for PIN translation/validation; encrypted PIN blocks; key rotations (ZMK/ZPK).
- **Switch Resilience**: Dual leased lines/VPNs, heartbeat monitors, hot-hot acquirer stack.

---

## 📊 Certification & Readiness Flow
1. **Connectivity** (VPN/MPLS, certs, mTLS).
2. **Functional test suites** (positive/negative, timeout/retry/idempotency).
3. **Security** (VA/PT, crypto conformance, key ceremonies).
4. **Ops drills** (failover, reconciliation breaks, complaint lifecycle).
5. **Go-Live sign-offs** (NPCI + internal Risk/IS).

---

## ✅ NPCI Checklist (Go-Live)
- [ ] Message adapters pass full cert suite (UPI/BBPS/IMPS/RuPay).
- [ ] mTLS + payload crypto + replay protection verified.
- [ ] Idempotency store + duplicate detection.
- [ ] Reconciliation job + break report + maker-checker flow.
- [ ] Complaint SLA timers + dashboards + audit logs.
- [ ] HSM: keys loaded, dual control, rotation calendar.
- [ ] Runbooks: switch failover, issuer timeouts, partial refunds.

---

## 🧪 Interview Q&A
**Q1. Ensure idempotency with UPI/IMPS under retries.**  
**A**: Use (RRN, STAN, amount, payee) as **idempotency key**; de-dupe cache (TTL = retry window); saga with compensations; store final state to prevent double-spend.

**Q2. BBPS reconciliation design?**  
**A**: Streaming ingest of BBPS files → staging → **matching engine** (rules per biller) → breaks to workflow queue → **maker-checker** adjustments; immutable ledger + audit trail; daily T+1 settlement report.

---

## 🧩 Case Prompt (Tailored)
Narrate your **BOU build**: certification, complaint management SLAs, recon dashboards, and how you instrumented **RRN-based tracing** end-to-end.
