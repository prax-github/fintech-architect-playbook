# Fintech Compliance Interview Playbook  
## File: 05_Compliance_Supporting_Patterns.md  

---

## 🎯 Purpose  
This file summarizes **architecture patterns** that make fintech systems compliant, resilient, and audit-ready.  
Each pattern links to compliance mandates such as PCI-DSS, GDPR, RBI, SEBI, IRDAI, SOX, and FFIEC.  

---

## 🧩 Anti-Corruption Layer (ACL)  

### Why  
- Isolates internal domain models from external/legacy/regulatory systems.  
- Prevents **data pollution** and enforces compliance boundaries.  

### How  
- Implement **adapter services** that translate external schema → internal canonical events.  
- Add **validation, masking, and enrichment** before persistence.  

### Example  
- **NPCI BBPS** integration → ACL service normalizes NPCI API fields into internal billing domain objects while applying **masking** and **audit logging**.  

---

## 🧩 Event Sourcing & CQRS  

### Why  
- Regulators (RBI, SEBI, SOX) demand **complete, immutable transaction history**.  
- CQRS supports separation of **read models (reports)** and **write models (ledgers)**.  

### How  
- Store every change as an **event** in an append-only log.  
- Reconstruct state on demand.  
- Provide **audit-ready query views** for compliance reporting.  

### Example  
- **SEBI trade logs** stored as event streams → auditors replay history.  
- **AML monitoring** reads from CQRS query store for suspicious transaction detection.  

---

## 🧩 Encryption Patterns  

### At Rest  
- AES-256, envelope encryption, KMS/HSM custody.  
- WORM storage for trade/order logs.  

### In Transit  
- TLS 1.2/1.3 with mTLS for service-to-service calls.  
- Signed requests (JWS) + encrypted payloads (JWE).  

### Example  
- **PCI-DSS vault** for card data → PAN encrypted with HSM, tokens exposed downstream.  
- **GDPR RTBF** → tombstone encrypted data + revoke keys to render inaccessible.  

---

## 🧩 Zero Trust Architecture  

### Why  
- Mandated by RBI, PCI-DSS, FFIEC to enforce **“never trust, always verify”**.  

### How  
- Enforce authentication/authorization **on every request**.  
- Service Mesh + OPA for continuous policy enforcement.  
- Context-aware ABAC (role + purpose + location).  

### Example  
- UPI API Gateway enforces **mTLS + JWT scopes**; backend microservices independently validate purpose claims before processing.  

---

## 🧩 Token-Based Authentication  

### JWT (JSON Web Token)  
- Signed (RS256/ES256), short-lived; carries claims for role, consent, purpose.  
- Enforces **scope-limited access** to APIs.  

### OAuth2 / OpenID Connect  
- Standard for fintech Open APIs (PSD2, UPI third-party apps).  
- Supports delegated consent → audit who granted access to what.  

### Example  
- **PSD2 PISP** uses OAuth2 flow → token contains consent ID → validated at gateway + PDP.  

---

## ✅ Compliance Patterns Checklist  

- [ ] ACL implemented for all legacy/regulatory integrations.  
- [ ] Event Sourcing in place for transaction history.  
- [ ] CQRS query models for audit/regulatory reporting.  
- [ ] AES-256 + TLS 1.3 + HSM-backed key management.  
- [ ] Zero Trust with mTLS + PDP enforcement.  
- [ ] Token-based auth (OAuth2/JWT) with scopes and consent IDs.  

---

## 🧪 Interview Q&A  

**Q1. How does Event Sourcing help with compliance?**  
**A**: Provides a **complete audit trail** of every state change; auditors can replay history; immutable log satisfies RBI/SEBI/SOX requirements.  

**Q2. How does ACL improve compliance posture?**  
**A**: Decouples external schema/requirements → internal domain. Prevents non-compliant data from entering core; allows masking and validation before persistence.  

**Q3. Why Zero Trust in fintech?**  
**A**: Regulators expect defense-in-depth. Zero Trust enforces authentication/authorization on every hop; prevents insider lateral movement; aligns with FFIEC cybersecurity expectations.  

**Q4. How do JWTs support GDPR/PSD2 compliance?**  
**A**: JWT claims can include **purpose & consent IDs**; ABAC policies validate them at runtime, ensuring **purpose limitation** and consent-bound access.  

---

## 📚 References  

- [Microservices Patterns – Audit Logging & ACL](https://microservices.io/)  
- [NIST Zero Trust Architecture](https://csrc.nist.gov/publications/detail/sp/800-207/final)  
- [PCI-DSS Tokenization Guidelines](https://www.pcisecuritystandards.org/)  
- [OpenID Connect](https://openid.net/connect/)  
- [SEBI Cybersecurity Circulars](https://www.sebi.gov.in/)  

---
