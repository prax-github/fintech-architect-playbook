# Fintech Compliance Interview Playbook  
## File: 02_Data_Protection_and_Privacy.md  

---

## 🎯 Purpose  
This file covers **data protection & privacy controls** critical in fintech systems.  
It links **compliance mandates → technical design choices** around encryption, tokenization, residency, and access control.  

---

## 🔐 Encryption (At Rest & In Transit)  

### At Rest  
- Use **AES-256** or stronger for databases, object stores, backups, and ledgers.  
- **Envelope encryption**: Master keys in HSM/KMS, DEKs rotated frequently.  
- Disk-level encryption (EBS, RDS, S3 SSE) + application-layer crypto for sensitive fields.  
- Immutable backups with client-side encryption where regulatory bodies demand dual assurance.  

### In Transit  
- Enforce **TLS 1.2/1.3** with strong ciphers, HSTS, Perfect Forward Secrecy.  
- Mutual TLS for inter-service traffic in sensitive zones.  
- Signed payloads (JWS) and encrypted payloads (JWE) for critical API calls.  
- Downgrade prevention: disable weak ciphers, SSL, TLS <1.2.  

**Architecture Patterns**:  
- API Gateway termination with TLS 1.3 → Service Mesh (mTLS).  
- Key rotation policies enforced via cloud KMS + automated detectors.  

---

## 🔑 Tokenization & Masking  

- **Tokenization**: Replace sensitive data (PAN, Aadhaar, SSN) with non-sensitive tokens.  
  - Vault-based (centralized service with HSM).  
  - Vaultless/deterministic (format-preserving, used in analytics).  
- **Masking**: PAN → `**** **** **** 1234`. Aadhaar → `XXXX-XXXX-1234`.  
- **Use Cases**:  
  - PCI-DSS: Tokenize PAN early to minimize CDE scope.  
  - RBI/NPCI: Mask identifiers in logs, reconcile via RRN/Stan instead of full PAN.  
  - GDPR/CCPA: Pseudonymize user identifiers for analytics pipelines.  

**Architecture Patterns**:  
- **Tokenization Service** → isolated, HSM-backed, audited.  
- **Masking Middleware** → strips/masks before logs, BI, or error traces.  

---

## 🌍 Data Residency & Sovereignty  

- **RBI/NPCI**: Full payments data must remain in India.  
- **GDPR**: EU citizen data must stay in EU, cross-border transfers need SCCs.  
- **CCPA**: Requires disclosure if data is sold/shared with non-US vendors.  

**Architecture Patterns**:  
- **Region-aware storage**: geo-fenced buckets, RDS clusters pinned to regulatory region.  
- **Multi-region design**: Dual active-active DR but **within same country** if mandated.  
- **Data Flow Governance**: DLP at egress, policy-as-code to enforce location tagging.  
- **Hybrid model**: Local PII vault + global pseudonymized IDs for analytics.  

---

## 🛡️ RBAC, ABAC & Zero Trust  

### RBAC (Role-Based Access Control)  
- Roles: Admin, Ops, Auditor, Customer, API Client.  
- Enforce **least privilege**; separate duties (SoD).  
- Periodic role/access reviews.  

### ABAC (Attribute-Based Access Control)  
- Policies based on **user, resource, environment** attributes.  
  - Example: `ClaimsAdjuster` can view `ClaimDocs` **if region=IN and consent=valid**.  
- Helps implement GDPR’s **purpose limitation**.  

### Zero Trust  
- **Principle**: Never trust, always verify.  
- Enablers: mTLS between services, continuous authentication/authorization, device posture checks, anomaly-based session risk.  
- Strong alignment with **RBI/PCI-DSS** “defense in depth”.  

**Architecture Patterns**:  
- Policy Decision Point (PDP, e.g., OPA) with dynamic ABAC rules.  
- Service Mesh enforcing mTLS + JWT claims.  
- JIT privileged access + break-glass workflows with approvals.  

---

## ✅ Checklist for Data Protection & Privacy  

- [ ] AES-256 at rest (DB, object store, backups).  
- [ ] TLS 1.2/1.3 in transit, mutual TLS for service-to-service.  
- [ ] Centralized tokenization vault, HSM-backed keys.  
- [ ] Masking applied in logs, BI exports, error traces.  
- [ ] Residency policies enforced (geo-fenced storage, DLP at egress).  
- [ ] RBAC roles defined, ABAC policies codified, periodic reviews in place.  
- [ ] Zero Trust patterns: mTLS, PDP-based decisions, JIT access.  

---

## 🧪 Interview Q&A  

**Q1. How would you implement encryption at rest in a multi-tenant payment system?**  
**A**: Use envelope encryption: per-tenant DEKs, master keys in HSM; tenant isolation enforced by unique key IDs; automated rotation; encrypted backups; transparent decryption at runtime.  

**Q2. How do you reduce PCI scope using tokenization?**  
**A**: Tokenize PAN at ingress via vault service → downstream services only see tokens; segregate vault in CDE; no PAN in logs or BI; scope reduced to smallest boundary.  

**Q3. How do you enforce RBI data localization in a cloud-native system?**  
**A**: Pin DBs/buckets to India regions; disable cross-region replication; geo-tagging for all data; DLP rules block egress; DR across two Indian seismic zones; prove compliance via config scans + evidence artifacts.  

**Q4. How does ABAC help with GDPR compliance?**  
**A**: Purpose tags in JWT/OPA → ABAC policies enforce that data access is allowed only if request purpose matches consent; dynamic enforcement ensures privacy-by-design.  

---

## 📚 References  

- [NIST Cryptographic Standards](https://csrc.nist.gov/projects)  
- [PCI-DSS Tokenization Guidelines](https://www.pcisecuritystandards.org/)  
- [RBI Data Localization](https://rbi.org.in/)  
- [GDPR Principles](https://gdpr-info.eu/)  
- [Zero Trust Architecture (NIST SP 800-207)](https://csrc.nist.gov/publications/detail/sp/800-207/final)  

---
