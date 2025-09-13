# ⚡ Quickstart

Welcome to the Gooclaim Developer Platform.  
This guide helps you run your first workflow with our **RCM Automation REST APIs** in a secure sandbox environment.

---

## 1️⃣ Request Access
Gooclaim is currently available to **pilot partners and EHR integrations**.  
To begin testing:  
- Email **support@gooclaim.com**  
- Our team will provide **sandbox credentials** (API key or service account).  

---

## 2️⃣ Environment URLs
- **Sandbox (no PHI):** `https://sandbox.api.gooclaim.com`  
- **Production (restricted):** `https://api.gooclaim.com`  

👉 Always begin in **Sandbox** to validate workflows before moving to production.

---

## 3️⃣ Your First Workflow — Coding API
Submit a clinical note or `DocumentReference` → receive ICD-10 and CPT codes with confidence scores and an audit trail.

---

### Example: curl
```bash
curl -X POST https://sandbox.api.gooclaim.com/v1/coding \
  -H "Authorization: Bearer SANDBOX_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "patient": {"id": "test-patient-123"},
    "documentReference": {"id": "doc-001"},
    "inputs": {
      "noteText": "Patient presents with chest pain, EKG performed."
    }
  }'
```

### Example Python (requests)
```bash
import requests

url = "https://sandbox.api.gooclaim.com/v1/coding"
headers = {
    "Authorization": "Bearer SANDBOX_API_KEY",
    "Content-Type": "application/json"
}
payload = {
    "patient": {"id": "test-patient-123"},
    "documentReference": {"id": "doc-001"},
    "inputs": {"noteText": "Patient presents with chest pain, EKG performed."}
}

response = requests.post(url, json=payload, headers=headers)
print(response.json())
```

###Example Response
```bash
{
  "id": "job_12345",
  "status": "completed",
  "codes": [
    {"system": "ICD-10-CM", "code": "I20.0", "confidence": 0.92},
    {"system": "CPT", "code": "93000", "confidence": 0.88}
  ],
  "audit": {
    "explanations": [
      {"segment": "EKG performed", "rationale": "Supports CPT 93000"}
    ]
  }
}

```

### Other REST APIs

Once you’ve tested Coding, try additional endpoints:

```bash 
POST /v1/prior_auth → Automates prior auth requests & tracking
```

```bash
POST /v1/denial_prediction → Flags high-risk claims before submission
```

```bash
POST /v1/appeals → Generates evidence-backed appeal letters
```

```bash
Each API returns structured JSON with confidence scores, explanations, and audit logs.
```

Next Steps

🔗 Integrations
 — Connect with Epic, Cerner, Athena via FHIR
📚 API Reference
 — Full endpoint list
🛡️ Compliance
 — HIPAA & SOC-2 foundations
