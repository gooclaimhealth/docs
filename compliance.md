# 🛡️ Security & Compliance

Gooclaim is built **compliance-first**, ensuring that patient data is handled with the highest standards of privacy, security, and auditability.  
Every API call is executed as a **Temporal workflow**, providing end-to-end traceability.

---

## 🔒 Data Protection

- **Encryption in Transit:** TLS 1.2+  
- **Encryption at Rest:** AES-256  
- **Key Management:** Managed KMS with rotation policies  
- **Data Minimization:** Only required PHI is processed, never stored unnecessarily  

---

## 🧾 Audit & Logging

- **Workflow Logging:** Each API call maps to a Temporal workflow with `workflowId` + `runId` for full traceability  
- **Audit Trails:** All data access and modifications logged with timestamps & user/service identity  
- **Redaction Pipeline:** PHI redacted in logs and monitoring systems  
- **Immutable Logs:** Tamper-evident storage for compliance audits  

---

## 👥 Access Control

- **Role-Based Access Control (RBAC):** Granular permissions for users and services  
- **Principle of Least Privilege:** Access scoped by role and workflow requirements  
- **MFA & SSO:** Multi-factor authentication and enterprise SSO (SAML/OIDC) supported  
- **API Scoping:** Tokens provisioned with restricted scopes (e.g., read-only vs. write)  

---

## 📜 Regulatory Alignment

- **HIPAA:** Safeguards for PHI in line with HIPAA Privacy & Security Rules  
- **SOC 2 (Type II):** Controls for security, availability, and confidentiality (certification in progress)  
- **GDPR & CCPA:** Data subject rights respected; deletion & export processes supported  
- **ABDM (India):** Alignment with Ayushman Bharat Digital Mission standards (roadmap)  

---

## 🔐 Infrastructure Security

- **Cloud Hosting:** Hosted in HIPAA-compliant cloud regions (Azure/AWS)  
- **Isolation:** Separate sandbox & production environments  
- **Network Security:** VPC isolation, firewalls, and zero-trust networking  
- **Monitoring:** 24/7 intrusion detection, vulnerability scans, and incident response  

---

## ✅ Best Practices for Partners

- Always test in **Sandbox** with non-PHI data  
- Rotate API keys regularly; use **short-lived tokens** where possible  
- Register **Webhooks** securely with HMAC signature verification  
- Store **workflowId** and `request_id` for audit readiness  
- Implement **data retention policies** consistent with your compliance program  

---

## 📅 Current Status

Gooclaim’s compliance program is **live and evolving**:  

- ✅ Encryption standards implemented (TLS 1.2+, AES-256)  
- ✅ RBAC, audit logging, and PHI redaction pipelines under development  
- 🟡 HIPAA & SOC 2 Type II certification **in progress**  
- 🟡 ABDM (India) alignment planned for future deployment  

---

## 📞 Compliance Inquiries

For security questionnaires, BAAs, or compliance-related inquiries:  
📧 **compliance@gooclaim.com**  
