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

✅ All tests passing (pytest)
✅ Security verified
✅ Reviewed and production-ready
