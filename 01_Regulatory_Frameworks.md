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
### 5. **DPDP (Digital Personal Data Protection Act, 2023 — India)**  
- **Scope & Roles**: Applies to *digital personal data* (and digitized data) processed in India or by Indian Data Fiduciaries/Processors; creates the **Data Protection Board of India** (DPB). **Data Principal rights** include access, correction, erasure, grievance redressal, and nomination. :contentReference[oaicite:0]{index=0} :contentReference[oaicite:1]{index=1}  
- **Core Obligations (high-impact)**:  
  - **Consent & Withdrawal** with comparable ease; fiduciary remains responsible for processors. :contentReference[oaicite:2]{index=2}  
  - **Security safeguards** and **breach intimation** to the **Board and each affected Data Principal**; retention only as needed → then **erase** (and cause processors to erase). :contentReference[oaicite:3]{index=3}  
  - **Children (<18)**: **verifiable parental consent**; **no tracking/behavioural monitoring/targeted ads** to children; avoid detrimental processing. :contentReference[oaicite:4]{index=4}  
  - **Significant Data Fiduciary (SDF)**: **DPO in India**, **independent data auditor**, **periodic DPIA & audits**. :contentReference[oaicite:5]{index=5}  
  - **Cross-border transfers**: permitted **except to countries the government notifies as restricted**. :contentReference[oaicite:6]{index=6}  
  - **Penalties**: DPB may levy monetary penalties **up to ₹250 crore** for specified contraventions. :contentReference[oaicite:7]{index=7}

- **Architecture implications**:  
  - **Consent/Purpose**: Consent registry; purpose tags in schemas/JWT; ABAC at a PDP (e.g., OPA) to enforce **purpose limitation**. :contentReference[oaicite:8]{index=8}  
  - **Rights Ops**: DSAR/RTBF orchestration (discover → render → erase/tombstone), with lawful-retention carve-outs. :contentReference[oaicite:9]{index=9}  
  - **Children’s Data**: Age-assurance + parent consent flows; **disable tracking/targeted ads** paths for child cohorts. :contentReference[oaicite:10]{index=10}  
  - **Security & Breach**: Policy-as-code for safeguards; breach runbooks to notify DPB + users in prescribed manner. :contentReference[oaicite:11]{index=11}  
  - **Cross-Border**: Egress DLP + country allow/deny lists mapped to Section 16 notifications. :contentReference[oaicite:12]{index=12}  
  - **SDF Readiness**: Org chart for **DPO (India-based)**, contract an **independent data auditor**, schedule **DPIA** cadence. :contentReference[oaicite:13]{index=13}

- **Checklist (DPDP)**  
  - [ ] Consent UI/SDK + withdrawal parity implemented. :contentReference[oaicite:14]{index=14}  
  - [ ] Purpose tags enforced at gateway/PDP; access decisions logged. :contentReference[oaicite:15]{index=15}  
  - [ ] RTBF + correction flows with evidence; processor erasure cascades. :contentReference[oaicite:16]{index=16}  
  - [ ] Child-data paths: age-gate, parental consent, **no tracking/ads**. :contentReference[oaicite:17]{index=17}  
  - [ ] Breach notification pipeline to **DPB + affected users**. :contentReference[oaicite:18]{index=18}  
  - [ ] Cross-border guardrails per govt. notifications. :contentReference[oaicite:19]{index=19}  
  - [ ] If **SDF**: DPO (India), data auditor, DPIA, periodic audits. :contentReference[oaicite:20]{index=20}

- **Interview Q&A (DPDP quick hits)**  
  **Q:** *How do you enforce purpose limitation under DPDP?*  
  **A:** Put purpose in consent + JWT; evaluate at a central PDP (OPA) on every call; log decisions and auto-revoke on consent withdrawal. :contentReference[oaicite:21]{index=21}  

  **Q:** *What changes for children’s data?*  
  **A:** Age-assurance + **verifiable parental consent**; block **tracking/behavioural monitoring/targeted ads**; add “detriment” checks in risk engine. :contentReference[oaicite:22]{index=22}
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
