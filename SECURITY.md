# 🔐 Security Policy — Sovereign AI Q&A Microservice

This document outlines the security controls implemented in this project as part of the **SOAISEC Internship** requirements.

The system is designed using a **defense-in-depth security model**, ensuring layered protection against common attack vectors such as unauthorized access, abuse, prompt injection, and credential leakage.

---

## 1️⃣ Authentication & Authorization

| Mechanism | Details |
|----------|---------|
| API Key Authentication | Required for all protected endpoints |
| Header Enforcement | `x-api-key` header must be included |
| Key Validation | Server-side verification (middleware) |
| Key Storage | Loaded securely from environment variables |
| No Hardcoding | Keys are not committed to source control |

Unauthorized requests return **401 Unauthorized**.

---

## 2️⃣ Input Security

| Protection | Description |
|-----------|-------------|
| Prompt Injection Filtering | Regex scanning blocks dangerous patterns |
| Length & Format Validation | Required `"question"` field |
| Strict JSON Parsing | Avoids malformed payload exploitation |

Blocked inputs trigger **400 response** with reason.

---

## 3️⃣ Output Security

| Issue | Mitigation |
|------|------------|
| LLM hallucination leakage (secrets) | Sensitive key regex scanning masks response |
| Data minimization | Only necessary fields returned to client |

If a response violates filter rules: 
`json
{"answer": "***blocked***", "blocked": true} `


---

## 4️⃣ Rate Limiting & Abuse Prevention

| Component | Setting |
|----------|---------|
| SlowAPI | 10 requests per minute per IP |
| Abuse detection | Incrementing counters for blocked/errors |
| DoS mitigation | 429 Too Many Requests response |

---

## 5️⃣ CORS & Network Security

| Setting | Purpose |
|--------|---------|
| Allowed origins restricted | Stops browser-based attacks |
| HTTPS recommended in production | Protects credentials & tokens in transit |

CORS is configured to avoid open wildcard access.

---

## 6️⃣ Secrets Management

| Control | Status |
|--------|-------|
| `.env` ignored by Git | ✅ |
| Server variables injected securely | ✅ |
| No plaintext keys in logs | ✅ |

Example `.env`:



---

## 7️⃣ Logging & Monitoring

| Data | Purpose |
|-----|---------|
| Structured JSON logs | Traceability & debugging |
| Metrics counters | Security analytics |

All logs are in-memory to avoid persistent leakage in testing environment.

---

## 8️⃣ Dependency Security

| Control | Status |
|--------|-------|
| Version pinning in `requirements.txt` | ✅ |
| No deprecated/abandoned libraries | ✅ |
| Minimal external dependencies | ✅ |

This mitigates supply-chain risks.

---

## ✅ Summary

This service is secured with:

- Authentication controls  
- Prompt & response filtering  
- Rate limiting  
- Safe key handling  
- CORS protection  
- Secure defaults  

It follows best practices for building **safe AI-assisted applications** in enterprise environments.

---

## 🔄 Disclosure of Vulnerabilities

If a security issue is discovered, contact the maintainers privately.  
Do **not** post vulnerabilities publicly until properly addressed.

---

Reviewer Navigation:

- `/api/health` → uptime status
- `/api/metrics` → security analytics
- `frontend/index.html` → interactive UI demo

Thank you for reviewing the project.

**Maintainer:**  
Shreyas — SOAISEC Security Internship Project  
2025
