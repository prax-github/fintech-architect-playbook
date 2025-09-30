# Fintech Compliance Interview Playbook  
## File: 01_Regulatory_Frameworks.md  

---

## 🎯 Purpose  
This file covers **regulatory frameworks** you should know as a fintech architect. It helps you answer interview questions around compliance mandates in payments, banking, and digital finance.

---

## 🇮🇳 Indian Regulations  

### 1. **RBI (Reserve Bank of India) Guidelines**  
- Regulates all payment systems, NBFCs, and banks.  
- Requires **two-factor authentication** for card transactions.  
- Enforces **data localization**: Payment data must be stored in India.  
- Regular **cybersecurity audits** for banks & payment providers.  

### 2. **NPCI (National Payments Corporation of India)**  
- Governs **UPI, BBPS, IMPS, RuPay**.  
- Requires certification & compliance testing for all participants.  
- Strict **API security & encryption** mandates.  
- Transaction logging & reconciliation rules.  

### 3. **SEBI (Securities and Exchange Board of India)**  
- Regulates fintechs dealing in securities, stockbroking, mutual funds.  
- Compliance with **KYC norms**, transaction reporting, and fraud detection.  

### 4. **IRDAI (Insurance Regulatory and Development Authority of India)**  
- Governs digital insurance platforms.  
- Strong rules for **customer data protection** & KYC for policies.  

---

## 🌍 Global Regulations  

### 1. **PCI-DSS (Payment Card Industry – Data Security Standard)**  
- Applies to all card transactions.  
- **Key mandates**:  
  - Encrypt PAN/cardholder data.  
  - No storage of CVV post-transaction.  
  - Quarterly vulnerability scans.  
  - Segregated cardholder data environment (CDE).  

### 2. **PSD2 (Payment Services Directive 2 – EU)**  
- Mandates **Strong Customer Authentication (SCA)**.  
- Open Banking APIs → Banks must expose APIs to licensed fintechs.  
- Consent-driven data sharing.  

### 3. **GDPR (General Data Protection Regulation – EU)**  
- Applies to any fintech handling EU citizen data.  
- **Key mandates**:  
  - Right to be forgotten (erasure requests).  
  - Data minimization (collect only what’s needed).  
  - Explicit user consent for data processing.  

### 4. **CCPA (California Consumer Privacy Act – US)**  
- Grants consumers right to:  
  - Opt-out of data sale.  
  - Request disclosure of personal data held.  
  - Request deletion of personal data.  

### 5. **SOX (Sarbanes-Oxley Act – US)**  
- Impacts financial reporting systems.  
- Requires **audit trails**, tamper-proof logs, and data integrity.  

### 6. **FFIEC (Federal Financial Institutions Examination Council – US)**  
- Provides compliance handbooks for banks.  
- Focus on **cybersecurity, fraud prevention, and risk management**.  

---

## ✅ Interview-Ready Notes  

- Always link **regulation → architecture decision**.  
  - Example: *PCI-DSS → encryption + tokenization + vault*.  
  - Example: *RBI data localization → region-aware data storage*.  
  - Example: *GDPR → build erasure APIs & consent flows*.  

- Expect interviewers to ask:  
  - “How would you design a **PCI-DSS compliant payment gateway**?”  
  - “What changes would you make for **GDPR right-to-erasure**?”  
  - “How do **RBI regulations** affect fintech system design in India?”  

---

## 📚 References for Deep Dive  

- [PCI-DSS Official Docs](https://www.pcisecuritystandards.org/)  
- [PSD2 Explained](https://www2.deloitte.com/insights/psd2.html)  
- [GDPR Summary](https://gdpr-info.eu/)  
- [RBI Guidelines](https://rbi.org.in/)  
- [NPCI Compliance](https://www.npci.org.in/)  

---
