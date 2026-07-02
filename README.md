# IELTSMate: AI Governance & Operations Architecture
> **Status:** Proprietary / Private Enterprise Project  
> **Note:** This repository serves as an architectural case study and governance blueprint. The actual source code is private and under NDA. This documentation outlines the system design, AI meta-rules, and operational workflows engineered for a multi-provider AI SaaS platform.

[![System Design](https://img.shields.io/badge/System-Design-blue?style=for-the-badge)](#)
[![AI Governance](https://img.shields.io/badge/AI-Governance-green?style=for-the-badge)](#)
[![Architecture](https://img.shields.io/badge/Infrastructure-Scalable-orange?style=for-the-badge)](#)

## 🏛️ Project Overview & Governance Philosophy

IELTSMate is a proprietary, enterprise-grade AI SaaS platform designed to deliver multi-modal language testing. Rather than treating LLMs as unpredictable black boxes, this platform was architected from the ground up with **strict AI governance, automated compliance, and operational resilience** at its core.

This case study highlights how complex, multi-provider AI pipelines can be governed using meta-rules, semantic caching, and self-healing background workflows to ensure 100% output compliance and cost-efficiency.

---

## 🧠 Core AI Governance & Meta-Rules

### 1. Prompt Version Control & Immutability
In production AI systems, prompts are infrastructure. The platform implements a strict governance layer for AI behaviors:
*   **Versioned Prompts:** All system prompts (scoring rubrics, tutor personas) are treated as versioned assets in the database.
*   **Instant Rollbacks:** Administrators can instantly roll back to previous prompt versions without server restarts if an AI behavior drifts from compliance standards.
*   **Audit Trails:** Every change to an AI meta-rule is logged with timestamps and admin attribution.

### 2. Automated Compliance & "Self-Healing" Workflows
To ensure generated content (lessons, feedback, datasets) meets strict quality, safety, and pedagogical standards, the platform utilizes autonomous background governance:
*   **`selfHealingWorker`**: Runs on a continuous cron schedule to validate all ecosystem outputs for algorithmic bias, repetition, and cultural sensitivity.
*   **`correctionWorker`**: Automatically triggered upon validation failure to regenerate content with specific avoidance constraints, ensuring the ecosystem self-corrects without human intervention.

### 3. Semantic Caching & Cost Governance
To govern API costs and latency across a multi-provider pipeline (DeepSeek, Deepgram, Alibaba TTS), the platform implements a vector-based semantic cache:
*   **Vector Embeddings:** Inputs are embedded and stored in a vector database.
*   **Cosine Similarity Thresholds:** Before any LLM scoring call, the system checks the cache. If a near-identical input (≥ 0.92 similarity) exists, it returns the cached, pre-validated result, drastically reducing external API dependencies and enforcing cost meta-rules.

### 4. Hot-Swappable Provider Infrastructure
The AI routing layer abstracts the underlying LLM providers to ensure high availability and vendor independence.
*   Admins can switch the active provider (e.g., OPENAI ↔ QWEN) via the database configuration.
*   Changes take effect on the next request with **zero downtime**, ensuring the platform remains resilient even if a primary AI vendor experiences outages.

---

## ⚙️ Operational Workflows & Background Systems

The platform relies on a robust, asynchronous background operations layer to handle heavy operational loads and maintain system state:

| Worker Queue | Operational Function | Governance Purpose |
| :--- | :--- | :--- |
| **`gen-worker`** | Automated generation of testing passages. | Ensures continuous content pipeline without manual bottlenecks. |
| **`self-heal`** | Continuous validation of ecosystem outputs. | Enforces quality and bias compliance meta-rules. |
| **`retention`** | Manages user lifecycle and spaced-repetition. | Governs user engagement workflows. |
| **`ft-export`** | Anonymizes and exports high-confidence data (JSONL). | Ensures strict data privacy compliance for LLM fine-tuning. |
| **`mc-validator`** | Hourly checks on draft courses for ambiguity/bias. | Continuous auditing of educational content. |

*Resilience Architecture:* If the message broker (Redis) is unavailable, workers gracefully degrade and execute logic inline, ensuring platform resilience in messy, real-world environments.

---

## 📊 Data Architecture & API Contracts

### Source-of-Truth API Design
To prevent frontend/backend drift and ensure strict type safety, all API contracts are defined first in an **OpenAPI 3.1 specification**. 
*   Frontend hooks are automatically generated from this source of truth.
*   This ensures seamless, compliant integration between frontend clients and background systems.

### Complex State & Multi-Tenancy
The data layer is designed for strict tenant isolation (B2B institutional accounts) and complex state management:
*   **JSONB Structuring:** Dynamic test details, grammar errors, and phoneme-level scoring are handled via JSONB to prevent schema bloat while maintaining queryability.
*   **Audit Trails:** Comprehensive activity and admin action logs ensure full operational transparency and accountability.

---

## 🛡️ Compliance, Security & Human-in-the-Loop

*   **Data Privacy (GDPR/PDPO):** Strict AI Data Processing consent gates block platform access until users explicitly accept data handling terms.
*   **Right to be Forgotten:** Native endpoints for full JSON data exports and soft-delete account anonymization.
*   **Human Review Queue:** A dedicated admin workflow allows human operators to intercept, manually grade, and override AI outputs, ensuring a safety net for edge cases and maintaining ultimate human governance over AI decisions.
*   **Idempotent Webhooks:** Payment webhooks are deduplicated via a dedicated database table to prevent state corruption or double-billing.

---

## 📫 Contact & Architecture Discussions

This architecture was designed and engineered by **Asad Haider**. 
I specialize in the intersection of AI systems architecture, technical governance, and regulatory compliance. 

*   **Email:** haiderasad2000@gmail.com
*   **Location:** Hong Kong 🇭🇰
