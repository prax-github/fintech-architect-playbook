# Fintech Compliance Interview Playbook
## File: 02B_PSD2_and_Open_Banking_EU_UK.md

## 🎯 Purpose
Build **PSD2/Open Banking** compliant architectures: SCA, consented data sharing, regulated TPP integrations, and API governance.

---

## 🧭 Roles & Terms
- **ASPSP**: Banks exposing APIs.
- **TPP**: AISPs (account info) / PISPs (payment initiation).
- **SCA**: Strong Customer Authentication (2+ of knowledge/possession/inherence).
- **Consent**: Explicit, scoped, time-bound user permission for access.

---

## 🔑 Requirements (Architecture Lens)
1. **Open APIs & Registration**
   - Conform to national/industry API profiles (e.g., UK OBIE).
   - **eIDAS/QWAC/QSeal** certs (EU) for TPP identification; dynamic client registration.

2. **SCA Flows**
   - Decoupled SCA preferred: app-to-bank approval (push/biometric).
   - Step-up based on risk (transaction value, device, behavior).

3. **Consent Management**
   - Central **Consent Store**: scopes, accounts, durations; revocation APIs; audit trails.
   - Enforce **least-privilege scopes** at runtime (OPA/ABAC).

4. **Secure Messaging**
   - mTLS, JWS signing of requests/responses; replay prevention; idempotency keys.

5. **Risk & Fraud**
   - Transaction risk analysis (TRA) with exemptions handling; dispute/chargeback pipelines.

6. **Operational**
   - Sandbox for TPPs, versioned APIs, change windows, KPIs uptime/error budgets; incident comms.

---

## 🏗️ Reference Architecture
- **API GW** (mTLS, JWS/JWE) → **Consent Service** (scopes, expiries)  
- **SCA Service** (push/OTP/biometrics) → **AuthN/Z** with dynamic policies  
- **Data Products**: Accounts, balances, transactions (pseudonymized IDs)  
- **PISP**: Payment initiation with PSU redirect/decoupled SCA; idempotency & status webhooks  
- **Observability**: consent ID & PSU ID tagging, secure audit logs

---

## ✅ Checklist
- [ ] Consent registry + revocation; scope enforcement in PDP (OPA).
- [ ] SCA coverage incl. exemptions with audit.
- [ ] mTLS + JWS; nonce/timestamp validation; idempotency store.
- [ ] Versioned OpenAPI specs; sandbox with synthetic data.
- [ ] Incident/availability KPIs reported per scheme rules.

---

## 🧪 Interview Q&A
**Q1. Implement SCA without harming UX.**  
**A**: Decoupled push (bank app biometric), risk-based exemptions (TRA), remember-device with risk caps, transparent fallbacks.

**Q2. Consent revocation impact.**  
**A**: Revoke at consent store → invalidate tokens/sessions; propagate cache purges; cease background jobs; retain audit trail.

**Q3. Prevent TPP replay.**  
**A**: mTLS + JWS; strict timestamp/nonce windows; one-time idempotency keys per PSU/TPP.

---

## 🧩 Design Prompt
Sketch a consent-centric architecture that supports AIS + PIS, including decoupled SCA, idempotent payments, and audit-ready logs.
