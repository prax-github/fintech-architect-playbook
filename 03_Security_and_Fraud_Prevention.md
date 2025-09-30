# Fintech Compliance Interview Playbook  
## File: 03_Security_and_Fraud_Prevention.md  

---

## 🎯 Purpose  
This file covers **security and fraud-prevention controls** critical in fintech systems.  
It maps **regulatory expectations → technical implementation** across identity, fraud, AML, and KYC.  

---

## 🔑 Authentication & Authorization  

### OAuth2 & OpenID Connect  
- OAuth2 for **authorization**; OpenID Connect for **authentication + identity federation**.  
- Issue **JWTs** (short-lived, signed, optionally encrypted).  
- Enforce scopes/claims at API gateways and service meshes.  

### MFA (Multi-Factor Authentication)  
- **Mandatory in RBI, PCI-DSS, PSD2** contexts.  
- Combine possession (device), knowledge (PIN), inherence (biometric).  
- Decoupled push-based MFA → better UX + compliance with PSD2 SCA.  

### Privileged Access  
- Just-In-Time (JIT) access; break-glass workflows with approvals.  
- PAM vaults for secrets, session recording for high-risk ops.  

**Architecture Patterns**:  
- Central **AuthN/Z service** integrated with PDP (OPA).  
- Identity provider federation (bank ID, Aadhaar eKYC, corporate SSO).  
- Service mesh enforcing JWT + mTLS per call.  

---

## 🔍 Fraud Detection  

### Velocity & Heuristics  
- Transaction frequency, value thresholds, device fingerprinting.  
- Typical rule: Block if >5 transactions in 30 seconds per card/device.  

### Behavioral Biometrics  
- Keystroke dynamics, geolocation, device posture.  
- Useful for UPI/PSD2 fraud risk exemptions (TRA).  

### AI/ML Anomaly Detection  
- Train models on **transaction graph + features** (amount, time, merchant, device).  
- Identify suspicious clusters (money mule rings, laundering).  

**Architecture Patterns**:  
- Streaming pipeline (Kafka → Flink/Spark) → rules engine + ML scoring.  
- Fraud decisions as **async side-car services** (don’t block core processing, but can trigger hold/alert).  
- Case management workflow for fraud analyst review.  

---

## 🏦 AML (Anti-Money Laundering)  

### Key Controls  
- **KYC at onboarding** → verify ID docs, biometric validation (UIDAI, PAN, SSN).  
- **PEP & Sanctions Screening** → OFAC, FATF, UN lists.  
- **Transaction Monitoring** → CTR (Cash Transaction Reports), STR (Suspicious Transaction Reports).  
- Risk-based thresholds: flag large transfers, cross-border anomalies.  

### Reporting & Audit  
- AML case workflows with SLA timers.  
- Immutable storage of alerts, evidence, investigator notes.  
- Integration with FIU-IND (India) or FinCEN (US).  

**Architecture Patterns**:  
- Real-time monitoring microservice → publish alerts → AML case queue.  
- Graph DB (Neo4j, TigerGraph) to model suspicious networks.  
- Audit logs WORM-protected for regulatory review.  

---

## 🧾 KYC (Know Your Customer) Workflows  

### eKYC (India)  
- Aadhaar-based OTP/biometric validation.  
- PAN/CKYC integration with bureaus.  
- Offline Aadhaar XML/QR for low-friction flows.  

### Global Variants  
- ID doc scanning + OCR + face match (US/EU).  
- Video KYC (India, EU pilot schemes).  
- AML screening at onboarding + periodic refresh.  

### Compliance Drivers  
- RBI, SEBI, IRDAI → enforce KYC for accounts, insurance, securities.  
- GDPR/CCPA → ensure consent + purpose limitation in KYC storage.  

**Architecture Patterns**:  
- **KYC Service**: manages workflows, integrates with bureaus/APIs.  
- **Document Vault**: encrypted storage with short-lived access URLs.  
- **Audit Trail**: store onboarding decisions, consent, and PEP screening logs.  

---

## ✅ Security & Fraud Checklist  

- [ ] OAuth2 + OIDC for all APIs; short-lived JWTs with claims.  
- [ ] MFA enabled for users + admins; decoupled push MFA for payments.  
- [ ] PAM for privileged accounts; break-glass workflows with audit.  
- [ ] Streaming fraud detection (rules + ML); case management system.  
- [ ] AML workflows: CTR/STR, sanctions list screening, FIU-IND/FinCEN reporting.  
- [ ] KYC orchestration: eKYC, doc vault, consent registry.  
- [ ] Immutable audit logs for all fraud/AML decisions.  

---

## 🧪 Interview Q&A  

**Q1. How would you design fraud detection for UPI/real-time payments?**  
**A**: Kafka streaming ingest → feature extraction → velocity rules + ML model → risk score. High-risk → step-up auth (MFA) or block; low-risk → auto-approve. Alerts routed to fraud analyst queue.  

**Q2. How do you ensure PSD2 Strong Customer Authentication (SCA) without hurting UX?**  
**A**: Implement decoupled push MFA (biometric approval in bank app); support TRA-based exemptions (low-value, trusted device). This balances compliance + customer experience.  

**Q3. How would you design AML transaction monitoring?**  
**A**: Ingest transactions into stream processor → rules engine (thresholds, blacklists) + ML → alerts pushed to AML case queue → investigators log evidence → CTR/STR reports generated. Immutable logs retained for regulators.  

**Q4. What’s the difference between KYC and AML?**  
**A**: KYC is **identity verification at onboarding**; AML is **ongoing monitoring of transactions**. Both are regulatory obligations, but AML is continuous whereas KYC is periodic refresh + onboarding.  

---

## 📚 References  

- [OAuth2 & OIDC Specs](https://openid.net/connect/)  
- [RBI AML/KYC Master Directions](https://rbi.org.in/)  
- [FATF AML Guidelines](https://www.fatf-gafi.org/)  
- [PCI-DSS MFA Requirements](https://www.pcisecuritystandards.org/)  
- [PSD2 SCA Explainer](https://www.eba.europa.eu/)  

---
