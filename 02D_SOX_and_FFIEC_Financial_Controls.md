# Fintech Compliance Interview Playbook
## File: 02D_SOX_and_FFIEC_Financial_Controls.md

## 🎯 Purpose
Build financial systems aligned with **SOX** (internal controls over financial reporting) and **FFIEC** (US banking IT/cyber guidelines): auditability, integrity, governance.

---

## 🔑 Control Themes → Engineering Actions
1. **Change Management & Segregation of Duties (SoD)**
   - Separate dev/test/prod; approval gates; artifact signing; reproducible builds; SBOMs.

2. **Access & Identity Governance**
   - Joiner-mover-leaver automation; periodic access certifications; PAM for privileged ops; session recording.

3. **Financial Data Integrity**
   - **Event-sourced ledgers**; double-entry accounting; reconciliation jobs with maker-checker; tie-outs to external statements.

4. **Logging & Evidence**
   - Immutable audit logs (WORM/append-only); time sync; retention policies; reportable controls mapped to dashboards.

5. **BCP/DR & Ops Resilience**
   - Tested DR; RTO/RPO controls; backup integrity checks; cybersecurity program aligned to risk appetite.

6. **Vendor/Third-Party Risk**
   - Due diligence, SLAs, right-to-audit, incident notification clauses, data flow diagrams maintained.

---

## 🏗️ Architecture Patterns
- **Ledger Microservice** with append-only journal, double-entry checks, and reconcile APIs.
- **Policy-as-Code** (Open Policy Agent) in CI/CD and runtime for SoD & approvals.
- **Evidence Pipelines**: auto-collect change tickets, scan reports, access logs → compliance data lake → dashboards.

---

## ✅ SOX/FFIEC Checklist
- [ ] SoD enforced in CI/CD; prod access via PAM; approvals logged.
- [ ] Ledger integrity checks; reconciliation breaks workflow with sign-offs.
- [ ] WORM logs & time-sync; retention documented.
- [ ] DR test artifacts; restore drills evidence.
- [ ] Vendor risk register; controls mapped; bridge letters where needed.

---

## 🧪 Interview Q&A
**Q1. How do you ensure financial data integrity?**  
**A**: Event sourcing + double-entry; deterministic posting rules; reconciliation to bank/processors; breaks handled via maker-checker with audit trails.

**Q2. What evidence do auditors expect?**  
**A**: Change approvals, access certs, VA/PT, DR drill results, reconciliations, SoD policy-as-code outcomes, immutable logs.

**Q3. Reduce audit toil?**  
**A**: Instrument controls → stream artifacts to a **compliance lake**; auto-generate control reports; alert on control drift.

---

## 🧩 Design Prompt
Propose an **evidence automation pipeline** that consolidates logs, tickets, scans, and approvals into auditor-ready reports.
