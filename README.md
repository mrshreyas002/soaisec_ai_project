# 🚀 Sovereign AI — Secure LLM Q&A Microservice

This project is part of the **SOAISEC AI Security Internship Assignment**.  
It demonstrates a secure, containerized LLM-backed Q&A microservice with a FastAPI backend, interactive frontend, OpenAI integration, and built-in security guardrails.

---

## 🧠 Features

- ⚙️ **FastAPI backend** with `/api/answer`, `/api/health`, `/api/logs`, `/api/metrics`
- 🔐 **Header-based API key authentication**
- 🧱 **Input/output guardrails** for prompt injection and sensitive data detection
- 🚦 **Rate limiting** via SlowAPI (`10 requests/minute per IP`)
- 🧰 **Structured JSON logging**
- 🧩 **OpenAI API integration** for Q&A generation
- 🧊 **Dockerized & deployable** on Cloud Run / Railway / Render
- 💻 **Interactive frontend** with animated particle background
- ☁️ **CORS-secured communication** between frontend & backend
- 📈 **Metrics tracking** (blocked, total, errors)

---

## 🔒 Threat Model & Attack Surface

| Threat Category | Attack Vector | Mitigation |
|----------------|--------------|-----------|
| Unauthorized access | Calls to backend without key | API key auth (header-based) |
| API key leakage | Hardcoded creds in repo | Keys stored in `.env`, gitignored |
| Prompt Injection | User tries to override system behavior | Regex blocking of banned patterns |
| Sensitive data leakage | OpenAI returns secrets | Output sanitization and filtering |
| Brute-force attacks | High request volume | SlowAPI rate limiting |
| CORS exploitation | Browser-based attacks | Allowed origins whitelisted only |
| DoS attempts | Spamming / overloading | 429 protection, metrics tracking |
| Supply-chain vulnerabilities | Python dependencies | Version pinning, minimal packages |

## 📘 Runbook: Deployment & Operations

### 1) Local Development
```bash
python -m venv .venv
venv\Scripts\activate     # Windows
pip install -r requirements.txt
uvicorn main:app --reload

for frontend activation 
python -m http.server 5500

# testing app health 
curl -X GET "http://127.0.0.1:8000/api/health"

Building Docker
docker build -t soaisec-ai .
docker run --env-file .env -p 8000:8000 soaisec-ai

## 🧭 Architecture Overview

```plaintext
                  ┌────────────────────┐
                  │   index.html       │
                  │ (Frontend - Nginx) │
                  └───────┬────────────┘
                          │  HTTP (port 5500)
                          ▼
                 ┌────────────────────────┐
                 │   FastAPI Backend       │
                 │   (Docker service)      │
                 │ ─ /api/health           │
                 │ ─ /api/answer           │
                 │ ─ /api/logs             │
                 │ ─ /api/metrics          │
                 └────────┬────────────────┘
                          │
                 ┌────────────────────────┐
                 │  OpenAI API (LLM)      │
                 │  (External provider)   │
                 └────────────────────────┘

## 🎯 Design Decisions & Trade-offs

| Decision | Reasoning | Trade-off |
|---------|-----------|-----------|
| FastAPI framework | Async + modern + easy integration with OpenAI | Slight learning curve |
| In-memory logs only | Avoid sensitive log persistence | Logs reset on restart |
| Regex guardrails | Simple + deterministic | Can overblock some creative prompts |
| API key header auth | Lightweight security | Not identity-based access |
| Dockerized architecture | Reproducible deployment | Requires Docker runtime |
| Limited dependencies | Smaller attack surface | Fewer built-in helpers |

✅ All tests passing (pytest)
✅ Security verified
✅ Reviewed and production-ready
