# PulseAgentZero — Entivia

Welcome to the **PulseAgentZero** organization, the architects behind **Entivia** — a production-grade, AI-driven behavioral intelligence platform built for the **[Hackathon Name]**.

## The Entivia Project

**Entivia** is a web-based multi-tenant intelligence platform that turns any business database into a continuously updating stream of decisions. It connects directly to a customer's operational database (Postgres, MySQL, MSSQL, SQLite), runs an autonomous multi-agent AI pipeline to score every entity — customers, accounts, devices, anything — and surfaces ranked, explainable next-best-action recommendations through a conversational dashboard. Entivia never copies the underlying data: it introspects the schema and queries live, so insights stay current and sensitive data never leaves the source.

### The Repositories

| Repository | Description |
| :--- | :--- |
| [**api**](https://github.com/PulseAgentZero/api) | The AI core. Multi-agent pipeline (Python/FastAPI) with a custom orchestrator, dual-LLM strategy, and per-tenant schema introspection. |
| [**dashboard**](https://github.com/PulseAgentZero/dashboard) | Real-time analyst dashboard (Next.js/React) — recommendations queue, entity profiles, live agent chat, and pipeline observability. |
| [**Docker Image**](https://hub.docker.com/r/chideraozigbo488/entivia) | Production-ready all-in-one image on Docker Hub — multi-arch (`linux/amd64`, `linux/arm64`), one-command self-hosting. |

---

## Problem Statement

Every business is sitting on rich operational data — transactions, support tickets, login events, billing history — but extracting *behavior* from it still costs months of data-warehouse work, a BI team, and a stack of dashboards no one opens. Existing tools stop at visualisation; they describe what happened but don't decide what to do next. And the few that try require moving data into yet another platform, which creates compliance friction and stale insights. This leaves SMEs and mid-market teams with:

- **Decision Latency** — by the time a churn or fraud signal makes it into a quarterly report, the customer is gone.
- **Static Dashboards** — BI tools answer the questions you already thought to ask; they don't surface the ones you missed.
- **Data Movement Risk** — every ETL into a third-party warehouse adds privacy, compliance, and reconciliation overhead.
- **Specialist Lock-in** — generating useful intelligence requires data engineers, ML engineers, and analysts most teams can't afford.

**Entivia** solves this by providing an **autonomous behavioral intelligence layer** that introspects your database, profiles every entity in it, scores risk and opportunity, and writes back actionable recommendations — all while leaving your data exactly where it is.

---

## Technical Execution

### The AI Core (Backend)
The backend is a custom **Agentic Pipeline Orchestrator** purpose-built for production reliability — no LangChain runtime in the hot path:
- **Four-Stage Autonomous Pipeline**: `SchemaIntelligenceAgent` → `ProfilingAgent` → `RiskScoringAgent` → `RecommendationAgent`, each a specialised ReAct-loop agent passing typed state through the chain.
- **Dual-LLM Resilience**: Anthropic Claude as primary with automatic fallback to Groq on rate limit / outage — every agent declares its model and tool set explicitly.
- **Conversational Agent with Live SQL Tools**: A separate chat agent backed by live, sandboxed queries against the customer's database — no hallucinated numbers, every answer is grounded in real-time SQL.
- **Multi-Tenant by Construction**: Every Postgres row carries an `org_id` injected from the JWT; cross-tenant access is impossible at the repository layer.
- **Encrypted at Rest**: Client database credentials are Fernet-encrypted before they ever touch the metadata store.

### The Platform (Frontend)
A modern, real-time dashboard built with **Next.js 14**:
- **Recommendations Queue**: Risk-tiered, explainable, one-click acknowledge / route-to-CRM.
- **Conversational Intelligence**: Stream the agent's reasoning, tool calls, and SQL results live via Server-Sent Events.
- **Pipeline Observability**: Watch the four-stage pipeline run, drill into any agent's reasoning trace, replay historical runs.
- **Connection & Schema Mapping**: Guided onboarding to map a customer's tables to Entivia's entity model — no SQL required.

### Infrastructure & DevOps
A **dual-mode deployment** that ships the same code to two very different topologies:
- **Cloud (Microservices)**: Six independently scaled services — `dashboard`, `api`, `agent`, `worker`, `scheduler`, `license` — behind nginx, sharing Postgres / Redis / Qdrant / S3.
- **Self-Hosted (All-in-One)**: A single `chideraozigbo488/entivia` image bundling every Entivia service + Redis + nginx, requiring only an external Postgres and Qdrant. Multi-arch built and published via Docker Buildx.
- **Vector Search**: Qdrant + Voyage AI embeddings for semantic entity lookup and RAG-augmented agent responses.
- **License & Billing**: Self-contained Paystack billing for cloud subscriptions and a JWT-signed license server for self-hosted Pro activations.

---

## The Team (PulseAgentZero)

We are a group of engineers dedicated to making behavioral intelligence accessible to every business — without the data-warehouse tax.

| Member | Role | GitHub Profile |
| :--- | :--- | :--- |
| **Abdulraqib20** | **AI Engineer** | [@Abdulraqib20](https://github.com/Abdulraqib20) |
| **Chideraozigbo** | **Data Platform Engineer** | [@Chideraozigbo](https://github.com/Chideraozigbo) |
| **mohawwal** | **Software Engineer** | [@mohawwal](https://github.com/mohawwal) |
| **Joy Ibe** | **Data Engineer** | [@JoyIbe](https://github.com/JoyIbe) |

---
*Created with care by the PulseAgentZero Team for the [Hackathon Name].*
