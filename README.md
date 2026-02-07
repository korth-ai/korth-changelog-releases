# Korth AI - Public Changelog

Welcome to the official changelog for the **Korth AI Trust Kernel**. This repository serves as a transparent record of our platform's evolution, security enhancements, and feature updates.

## 🚀 Recent Updates

### [v1.0.0] - 2026-02-07
- **Initial Public Release**: Official launch of the Korth AI Trust Kernel.
- **Security**: Implemented real-time failed login alerting on GCP.
- **Compliance**: Alignment with ISO 42001 (AI Governance) and SOC 2 frameworks.
- **Infrastructure**: Production environment stabilized in Doha (me-central2) region.

---
**Security Contact**: For vulnerability disclosures, please reach out to `security@korth.ai`.




# 🛡️ KORTH KERNEL
### AI Security Infrastructure & Middle-tier

[![Next.js](https://img.shields.io/badge/Frontend-Next.js%2014-black)](https://nextjs.org/)
[![Rust](https://img.shields.io/badge/Backend-Rust%20Axum-orange)](https://www.rust-lang.org/)
[![Security](https://img.shields.io/badge/Security-CSP%20Hardened-green)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)

**Korth AI** is a high-performance security infrastructure designed to intercept, analyze, and neutralize malicious AI prompts. By implementing a sophisticated **Uplink Security Model**, the system creates a defensive perimeter between users and LLMs.

---

## 🏗️ Architecture: Three-Layer Defense-in-Depth

Korth AI employs three independent security layers to ensure system resilience and low-latency threat detection.

### 1. Client-Side ThreatEngine (Neural Inference)
* **WASM-based Detection**: Utilizes `Transformers.js` to run neural network inference directly in the user's browser.
* **Zero-Latency Filtering**: Pre-screens prompts before they leave the client environment.
* **Sandboxed Execution**: Runs within a dedicated **Web Worker** via `blob:` URLs, governed by a custom **Content Security Policy (CSP)**.

### 2. Edge Security Gateway (Middleware)
* **DoS Protection**: Enforces a strict `10KB content-length` limit on high-computation routes.
* **Hybrid Authentication**: Validates requests through both native Supabase sessions and custom **JWT-based uplink tokens**.
* **Header Hardening**: Injects critical security headers (HSTS, No-Sniff, XSS Protection).

### 3. Semantic Analysis Engine (Rust Backend)
* **High-Concurrency Engine**: Built with **Rust (Axum)** and deployed on **Google Cloud Run**.
* **Pattern Matching**: Dynamically fetches malicious patterns from Supabase.
* **Secure DB Uplink**: Uses high-privilege `service_role` JWT keys for administrative auditing.

---

## 🔐 Security Configuration

### Content Security Policy (CSP)
Korth AI enforces a rigorous CSP via `next.config.js` to prevent XSS and unauthorized data exfiltration:

| Directive | Allowed Sources |
| :--- | :--- |
| **connect-src** | `'self'`, Backend URL, Supabase, `cdn.jsdelivr.net` |
| **script-src** | `'self'`, `'unsafe-eval'`, `cdn.jsdelivr.net` |
| **worker-src** | `'self'`, `blob:` |

> [!IMPORTANT]
> To support semantic filtering, the `malicious_patterns` table must include an `is_active` boolean field.

---

## 📡 Deployment Infrastructure

### Google Cloud Run (Backend)
* Deployed in **`me-central1`** for low-latency service in the UAE.
* Environment variables must use standardized **JWT formats** (`eyJ...`).

### Vercel (Frontend & Middleware)
* **Dynamic Rewrites**: Maps `/api/*` to the Cloud Run backend via `NEXT_PUBLIC_API_URL`.
* **Standalone Mode**: Optimized production footprint for maximum performance.

