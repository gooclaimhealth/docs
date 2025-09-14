# 📚 API Reference

Gooclaim exposes **REST APIs** for revenue cycle automation.  
Every request starts a **Temporal Workflow** in the background to ensure **reliability, retries, and auditability**.  

---

## 🔗 Base URLs

- **Sandbox (no PHI):** `https://sandbox.api.gooclaim.com`  
- **Production (restricted):** `https://api.gooclaim.com`  

👉 All endpoints are **POST** requests returning **JSON**.  

---

## 🔐 Authentication

Send your token in the Authorization header:  

```bash
Authorization: Bearer <YOUR_TOKEN>
Content-Type: application/json
```

- Tokens are provided during onboarding.  
- Temporal workflows log every request with a unique **workflowId / runId**.  

---

## 🧱 Conventions

- **Format:** JSON over HTTPS  
- **Status codes:**  
  - `202 Accepted` → Workflow started  
  - `200 OK` → Workflow result returned (for sync calls)  
  - `4xx` → Client error  
  - `5xx` → Server/Workflow error  
- **Idempotency:** Send `Idempotency-Key` header to prevent duplicate workflows  

---

## 📦 API Endpoints

### 1) Coding API
Analyzes clinical notes and returns ICD-10/CPT codes with audit trail.  

**Endpoint**  
`POST /v1/coding`  

**Request**
```json
{
  "patient": {"id": "PATIENT_ID"},
  "documentReference": {"id": "DOC_ID"},
  "inputs": {
    "noteText": "Patient presents with chest pain, EKG performed."
  },
  "options": {"returnAuditTrail": true}
}

```

```json
{
  "workflowId": "wf_coding_12345",
  "runId": "abcde-67890",
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

2) Prior Authorization API

Automates prior auth request creation and tracks status via Temporal workflows.

### Endpoint
```bash
POST /v1/prior_auth
```

### Request
```json
{
  "patient": {"id": "PATIENT_ID"},
  "procedures": [{"code": "93000", "system": "CPT"}],
  "diagnoses": [{"code": "I20.0", "system": "ICD-10-CM"}],
  "attachments": [{"uri": "s3://bucket/discharge-summary.pdf"}]
}
```
### Response
```json
{
  "workflowId": "wf_priorauth_789",
  "status": "in_progress",
  "priorAuth": {
    "id": "PA-2025-001",
    "status": "pending_review"
  }
}
```

3) Denial Prediction API

Predicts claim denial risk before submission.

### Endpoint

```bash
POST /v1/denial_prediction
```

### Response
```json
{
  "workflowId": "wf_denial_456",
  "status": "completed",
  "riskScore": 0.27,
  "factors": [
    {"type": "missing-doc", "weight": 0.4},
    {"type": "coding-ambiguity", "weight": 0.2}
  ],
  "recommendations": [
    "Attach EKG report",
    "Verify coverage for CPT 93000"
  ]
}
```

4) Appeals API

Generates an evidence-backed appeal letter for denied claims.

### Endpoint
```bash
POST /v1/appeals
```

### Response
```json
{
  "workflowId": "wf_appeal_321",
  "status": "completed",
  "appeal": {
    "draftLetter": {
      "mimeType": "application/pdf",
      "uri": "s3://bucket/appeals/PA-2025-001-draft.pdf"
    },
    "summary": "Appeal drafted citing medical necessity with attached EKG."
  }
}
```

📨 Webhooks

For long-running workflows, Gooclaim sends webhook events when a Temporal workflow completes.

Headers
X-Gooclaim-Signature: t=<ts>, v1=<hmac>

Payload

```json
{
  "event": "workflow.completed",
  "workflowId": "wf_coding_12345",
  "status": "completed",
  "result": { /* endpoint-specific output */ }
}
```

🛡️ Observability

Every API call maps to a Temporal Workflow with workflowId + runId.

Use 
```bash
GET /v1/workflows/{workflowId} to query status if webhooks are unavailable.
```
All executions are logged for compliance and audit trails.

