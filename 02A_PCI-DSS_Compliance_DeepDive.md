# Fintech Compliance Interview Playbook
## File: 02A_PCI-DSS_Compliance_DeepDive.md

## 🎯 Purpose
Architecture and ops guardrails to build/operate **PCI-DSS** compliant card data flows (issuers, acquirers, gateways, aggregators).

---

## 🧭 Scope & Applicability
- Any system that **stores, processes, or transmits** cardholder data (CHD) or sensitive authentication data (SAD).
- In-scope components: APIs, payment pages, vault/token service, databases, logs, backups, CI/CD, monitoring, support tools.

---

## 🔑 PCI-DSS Core Expectations (Architect’s View)
1. **Minimize CDE**
   - Strictly **segment** the Cardholder Data Environment (CDE) from everything else.
   - Push CHD handling into the **smallest possible footprint** (e.g., dedicated tokenization/vault).

2. **Data Protection**
   - **No CVV storage** post-authorization.
   - PAN: display **masked**, store **encrypted** (AES-256), tokenize wherever possible.
   - Strong TLS 1.2/1.3; HSTS; modern cipher suites.

3. **Access Control**
   - Role/attribute-based, **least privilege**, MFA for admins, JIT access, session recording in jump hosts.
   - Unique IDs for all users/services; rotate credentials/keys.

4. **Monitoring & Testing**
   - Centralized logging with tamper evidence; FIM (File Integrity Monitoring).
   - **Quarterly ASV scans**, **annual penetration tests**, frequent VA.
   - Daily key events alerts (auth failures, config drift, policy violations).

5. **Secure SDLC**
   - Threat modeling, SAST/DAST, dependency scanning, IaC policy checks, segregation of duties.

6. **Key Management**
   - KMS/HSM, key rotation & dual control; split knowledge for master keys.

---

## 🏗️ Reference Architecture (High-Level)
- **Edge**: WAF + mTLS API GW → **Payment UI** (JS SDK or hosted fields to avoid CHD in your app)  
- **Tokenization Service (CDE)**: HSM-backed PAN vault → returns **surrogate token**  
- **Payments Orchestrator** (out-of-CDE): uses tokens only, calls acquirer/processor  
- **Observability**: separate secured log plane, masked fields, PAN never logged  
- **Segmentation**: firewalled CDE VPC/VNET; separate CI/CD runners; jump boxes with MFA

---

## ✅ Engineer’s Checklist
- [ ] PAN never logged; logs masked at source.
- [ ] Tokenization before persistence; **vault isolated** with HSM.
- [ ] CDE network ACLs + micro-segmentation; separate build agents.
- [ ] Quarterly ASV, annual PT; remediation tracked to closure.
- [ ] FIM on critical paths; time-sync (NTP) everywhere.
- [ ] Access reviews, key rotations, break-glass process tested.
- [ ] Third-party due diligence for any CHD handler.

---

## 🧪 Interview Q&A
**Q1. How to reduce PCI scope?**  
**A**: Hosted fields/iFrame/JIT SDK to keep CHD off your servers; tokenize early; isolate vault in dedicated CDE; all other services use tokens.

**Q2. Where can PAN appear?**  
**A**: Only inside the **CDE** (vault/tokenization). Elsewhere: **surrogate tokens**; PAN must be masked if displayed.

**Q3. Prove “no CVV storage.”**  
**A**: Code scans, DB schema proofs, log sampling, PT results; automated content scans (DLP) across storage/logs; SDLC guardrails.

---

## 🧩 Design Prompt
Design a multi-region, tokenization-first gateway: describe CDE segmentation, HSM custody, and safe retries without duplicating charges.
