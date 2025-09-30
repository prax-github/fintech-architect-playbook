# Fintech Compliance Interview Playbook  
## File: 01_Regulatory_Frameworks.md  

---

## 🎯 Purpose  
This file covers **regulatory frameworks** a fintech architect must know.  
It maps **laws/regulations → system design decisions** so you can confidently answer interview questions on compliance in payments, banking, and financial systems.  

---

## 🇮🇳 Indian Regulations  

### 1. **RBI (Reserve Bank of India) Guidelines**  
- Regulates banks, NBFCs, wallets, PSPs, and payment aggregators.  
- **Key mandates**:  
  - **Two-Factor Authentication (2FA)** for card transactions.  
  - **Data localization** → All payments data must be stored in India.  
  - **Periodic IS audits, VA/PT**, and IT governance.  
  - **BCP/DR drills** with dual-site setups across seismic zones.  
- **Architecture implications**:  
  - Region-pinned DBs, backups, and HSM/KMS hosted in India.  
  - Event-sourced immutable audit logs for financial transactions.  
  - Zero Trust + JIT access for admins.  

---

### 2. **NPCI (National Payments Corporation of India)**  
- Governs **UPI, BBPS, IMPS, RuPay/NFS** ecosystems.  
- **Key mandates**:  
  - End-to-end encryption & digital signatures for API payloads.  
  - **Certification** required before go-live (UPI/BBPS/IMPS).  
  - Mandatory **transaction logging, reconciliation, and settlement (T+1)**.  
  - SLA-driven complaint management workflows.  
- **Architecture implications**:  
  - Message adapters for ISO-8583/UPI/BBPS formats.  
  - Reconciliation microservices + break-report dashboards.  
  - Circuit breakers, retry queues with **idempotency keys (RRN, STAN)**.  
  - HSM-backed key ceremonies for RuPay/NFS PIN security.  

---

### 3. **SEBI (Securities and Exchange Board of India)**  
- Regulates securities, brokers, wealth platforms, mutual funds.  
- **Key mandates**:  
  - **KYC via KRA** (KYC Registration Agencies).  
  - **Market surveillance** for suspicious trading (spoofing, pump & dump).  
  - **Immutable trade/order logs** with WORM storage.  
  - Investor grievance redressal (SCORES integration).  
- **Architecture implications**:  
  - Stream processing (Kafka/Flink) → real-time surveillance → case queues.  
  - Event-sourced OMS with notarized, hash-chained logs.  
  - Policy engine (OPA) for investor suitability checks.  

---

### 4. **IRDAI (Insurance Regulatory and Development Authority of India)**  
- Governs insurers, TPAs, brokers, and aggregators.  
- **Key mandates**:  
  - **Policyholder data protection** (PII + health data).  
  - **Consent & purpose limitation** for personal/medical data.  
  - **Claims SLAs** for processing, refunds, endorsements.  
  - AML/KYC checks on policyholders & POSPs.  
- **Architecture implications**:  
  - Consent registry + purpose-bound tokens.  
  - Claims case pipelines with SLA timers + secure object stores.  
  - Fraud detection across claims using rules + ML graph analytics.  

---

## 🌍 Global Regulations  

### 1. **PCI-DSS (Payment Card Industry – Data Security Standard)**  
- Applies to anyone handling cardholder data (CHD).  
- **Key mandates**:  
  - Encrypt PAN; **no CVV storage** post-transaction.  
  - Segment **Cardholder Data Environment (CDE)** from rest of infra.  
  - Quarterly **vulnerability scans & annual penetration tests**.  
  - Key custody via HSM with dual control.  
- **Architecture implications**:  
  - Early tokenization, PAN vault microservice in isolated VPC.  
  - Micro-segmentation + WAF + FIM (File Integrity Monitoring).  
  - Immutable audit logs for CDE access.  

---

### 2. **PSD2 (Payment Services Directive 2 – EU)**  
- Governs **open banking** APIs in EU.  
- **Key mandates**:  
  - **Strong Customer Authentication (SCA)** → 2FA (biometric, OTP, device).  
  - Banks must expose **standardized APIs** for AIS/PIS.  
  - Explicit **consent-driven data sharing** with TPPs.  
- **Architecture implications**:  
  - Consent store + revocation APIs.  
  - Decoupled SCA flows (bank app push approvals).  
  - mTLS + JWS/JWE for API requests/responses.  

---

### 3. **GDPR (General Data Protection Regulation – EU)**  
- Governs personal data of EU citizens.  
- **Key mandates**:  
  - **Right to be forgotten (RTBF)**.  
  - **Data minimization** → collect only what’s necessary.  
  - Explicit user **consent** for processing.  
- **Architecture implications**:  
  - DSAR/RTBF orchestration workflows.  
  - Purpose-tagged schemas enforced at PDP (e.g., OPA).  
  - Pseudonymization for analytics.  

---

### 4. **CCPA (California Consumer Privacy Act – US)**  
- Governs California residents’ data.  
- **Key mandates**:  
  - Right to **opt-out of data sale**.  
  - Right to disclosure + deletion of data.  
- **Architecture implications**:  
  - Profile flag → enforced in data pipelines (ads/analytics).  
  - DSAR APIs for disclosure/deletion.  
  - Audit logs of opt-outs & revocations.  

---

### 5. **SOX (Sarbanes-Oxley Act – US)**  
- Ensures integrity of financial reporting systems.  
- **Key mandates**:  
  - Strong **change management & segregation of duties (SoD)**.  
  - **Tamper-proof audit trails**.  
  - Periodic access reviews & certifications.  
- **Architecture implications**:  
  - Event-sourced ledgers with double-entry checks.  
  - Immutable audit logs (WORM).  
  - Compliance evidence automation pipelines.  

---

### 6. **FFIEC (Federal Financial Institutions Examination Council – US)**  
- Provides guidelines for US banking IT & cybersecurity.  
- **Key mandates**:  
  - **Cybersecurity assessments**.  
  - Fraud detection + transaction risk monitoring.  
  - DR/BCP & vendor risk frameworks.  
- **Architecture implications**:  
  - Fraud pipelines (rule + ML scoring).  
  - BCP drills with evidence logs.  
  - Vendor risk registers + third-party attestation storage.  

---

## ✅ Interview-Ready Notes  

- Always map **regulation → system design decision**. Examples:  
  - *RBI Data Localization* → India-region-only storage, geo-fenced backups.  
  - *NPCI UPI/BBPS* → Idempotency keys (RRN, STAN) + reconciliation microservices.  
  - *PCI-DSS* → Tokenization-first design + CDE segmentation.  
  - *PSD2* → Consent registry + decoupled SCA.  
  - *GDPR* → RTBF APIs + purpose-bound tokens.  
  - *SOX* → Immutable audit logs + evidence automation.  

- Common interview prompts:  
  - “How would you design a **PCI-DSS compliant payment gateway**?”  
  - “What design choices ensure **RBI localization compliance**?”  
  - “How do you support **GDPR right-to-erasure** in a microservices setup?”  
  - “What’s your approach to **audit trails under SOX/SEBI**?”  

---

## 📚 References for Deep Dive  

- [RBI Guidelines](https://rbi.org.in/)  
- [NPCI Compliance](https://www.npci.org.in/)  
- [SEBI Regulations](https://www.sebi.gov.in/)  
- [IRDAI Guidelines](https://irdai.gov.in/)  
- [PCI-DSS Official Docs](https://www.pcisecuritystandards.org/)  
- [PSD2 Explained](https://www2.deloitte.com/insights/psd2.html)  
- [GDPR Summary](https://gdpr-info.eu/)  
- [CCPA Summary](https://oag.ca.gov/privacy/ccpa)  
- [SOX Overview](https://www.sec.gov/spotlight/sarbanes-oxley.htm)  
- [FFIEC IT Handbook](https://ithandbook.ffiec.gov/)  

---
