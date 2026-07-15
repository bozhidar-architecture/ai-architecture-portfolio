---
title: Governance & Compliance
layout: single
toc: true
toc_sticky: true
description: AI governance reviews, architecture compliance automation, and responsible development practices for enterprise AI solutions.
---

Governance is where architecture delivers the most lasting value. While building AI solutions is exciting, ensuring they remain **responsible, secure, accessible, and legally compliant** is what separates production-grade work from prototypes.

> *In teams where AI-assisted development is the norm, governance isn't overhead — it's the guardrail that makes speed sustainable.*

---

## Governance Review Framework

Every AI solution I architect goes through a structured review covering five dimensions:

### 1. Responsible AI Review
- Fairness and bias assessment across model outputs
- Transparency requirements — can users understand why the AI responded as it did?
- Human oversight — where are the escalation points?
- Grounding validation — are responses grounded and citation-backed, with unsupported answers refused rather than guessed?
- Content filtering and safety boundaries

### 2. Open-Source & Licence Compliance
- Critical when development is AI-assisted (LLM-generated code may introduce copyleft or restrictive licences without developer awareness)
- Automated scanning of dependencies and generated code for licence compatibility
- Policy enforcement: approved vs. restricted licence categories
- Audit trail for AI-generated code contributions

### 3. Threat Modelling
- Attack surface analysis for AI-specific threats (prompt injection, data poisoning, model extraction)
- Authentication and authorisation review (e.g., managed identity vs. API keys)
- Data flow analysis — where does sensitive data travel, and is it encrypted in transit and at rest?
- PII handling and redaction review (logging, telemetry, conversation history)

### 4. Accessibility Review
- WCAG compliance for AI-powered interfaces (chat widgets, dashboards)
- Screen reader compatibility and keyboard navigation
- Cognitive accessibility — are AI responses clear and actionable?
- **A standing commitment** — I treat inclusive, WCAG-compliant AI interfaces as a default design target, not an afterthought.

### 5. Privacy Review
- Data residency and sovereignty requirements
- PII detection and redaction in logs, telemetry, and AI training data
- Consent and transparency — do users know they're interacting with AI?
- Right to erasure — can conversation history be purged?

---

## Architecture Drift Detection

One of the biggest governance challenges: **code drifts from agreed architecture** over time, especially in fast-moving AI projects.

### Approach
- **Architecture Decision Records (ADRs)** define the intended design
- **Automated compliance checks** compare the live codebase against architectural constraints
- **Drift reports** flag deviations — undeclared dependencies, bypassed security patterns, missing telemetry, unauthorised service usage
- **Continuous validation** integrated into CI/CD — not a one-time audit

### What Gets Checked

| Category | Examples |
|----------|----------|
| Security patterns | Managed Identity usage, no hardcoded secrets, CORS at infra level |
| Service boundaries | Only approved Azure services deployed, no shadow IT |
| Data handling | PII redaction in place, encryption at rest, logging policies followed |
| Code provenance | AI-generated code scanned for licence compliance |
| Observability | App Insights instrumented, structured logging, no PII in telemetry |

### Why This Matters
Developers often see governance as friction. The key is **making compliance automated and invisible** — checks run in the pipeline, not in a review meeting. When drift is caught early, the fix is small. When it's caught in production, it's an incident.

---

## Applied Governance

These governance practices are actively applied across portfolio projects:

- **[Network Platform RAG Assistant](../reference-architectures/#work-rag-assistant)** — STRIDE threat model including prompt injection, red-team test suite, PII-safe logging, and managed identity with private networking enforced by design
- **[Skills + MCP Triage Assistant](../reference-architectures/#work-triage-assistant)** — architecture-conformance checks against the ADR set; every external write gated to the engineer
- **[Software Release Impact Advisor](../reference-architectures/#work-release-advisor)** — domain-scoped refusal patterns and mandatory citations in every output
- **[Configuration Analyzer](../reference-architectures/#work-config-analyzer)** — privacy as the core feature: zero telemetry, in-browser processing, PII anonymisation built in

---

[← Back to Portfolio](../)
