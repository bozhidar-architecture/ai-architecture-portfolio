---
title: Reference Architectures
layout: single
toc: true
toc_sticky: true
toc_label: "Architectures"
description: AI reference architectures — declarative agents, RAG with deterministic validation, multi-agent skills + MCP triage, and privacy-first tooling for enterprise telco.
---

{% include mermaid-lightbox.html %}

*Four original reference designs, anonymised from real enterprise-telco work — each
presented as the decisions and trade-offs that shaped it, not just the boxes and lines.*

## 1. Declarative AI Agent: Software Release Impact Advisor {#work-release-advisor}
- **Scenario:** Telco operators upgrading network platform software need to understand new features, bug fixes, deprecations, and known issues across every intermediate release. This agent automates hours of manual document review and produces structured, citation-backed impact assessment reports.
- **Architecture Pattern:** Declarative agent (no runtime code) using RAG with hybrid retrieval (semantic + keyword search) and semantic re-ranking over vendor release documentation.
- **Key Design Decisions:**

  | Decision | Rationale |
  |----------|-----------|
  | No runtime code | Pure prompt engineering — the AI platform handles execution |
  | Strict domain scoping | Agent refuses out-of-scope queries to prevent misuse |
  | Mandatory output template | Consistent 5-section Markdown report for every response |
  | Version-aware retrieval | Semantic versioning used as a filter for precise document retrieval |
  | No-invention policy | Unknown values labelled explicitly — no hallucinated data |

- **Scope:** Led architecture and design within a cross-functional team.
- **Value Proposition:** Reduces upgrade impact analysis from hours of manual review to minutes, with full citation traceability. It replaced the previous manual process outright, and the same framework was reused to scale impact assessment across additional products.

## 2. Privacy-First Network Configuration Analyzer {#work-config-analyzer}
- **Scenario:** Telecom operators need to analyse network platform configuration files to detect active licensed features, identify missing configurations, and safely anonymise sensitive data before sharing configs with support teams or vendors.
- **Architecture Pattern:** Zero-dependency, fully offline static web application — pure client-side processing with no cloud backend, no telemetry, and no data leaving the browser. Designed for air-gapped environments.
- **Key Capabilities:**

  | Capability | Approach |
  |------------|----------|
  | SKU/Licence detection | Regex-driven pattern matching across 19 licensed feature signatures |
  | Configuration anonymisation | Masks all PII (IPs, hostnames, MACs, phone numbers) for safe sharing |
  | Feature analysis | Parses 18 config sections with visual indicators for configured vs. missing |

- **Key Design Decisions:**

  | Decision | Rationale |
  |----------|-----------|
  | Privacy-first | Zero network calls, no telemetry — everything runs in-browser |
  | No framework / no dependencies | Single-class vanilla JS — no CDNs, no npm, fully offline-capable |
  | Air-gapped distribution | Runs via file:// protocol or local HTTP server |
  | Progressive disclosure | URL parameter flags unlock advanced UI; default is MVP mode |
  | Regex-driven analysis | Deterministic, auditable pattern matching — no ML black box needed |

- **Scope:** Led architecture and development within a cross-functional team.
- **Why not AI?** This project demonstrates a critical architecture skill: knowing when AI is unnecessary. Pattern matching against known signatures is deterministic, auditable, and deployable in restricted environments where cloud AI services aren't an option.
- **Value Proposition:** Enables secure configuration sharing and feature auditing without exposing sensitive network data, deployable in restricted environments with zero infrastructure. It has been used for licensing true-ups and feature audits, and took on a life of its own — colleagues forked the first version to add new features.

## 3. Production RAG Assistant for Network Platform Support Engineering {#work-rag-assistant}
- **Scenario:** Support engineers and operators working with a mobile packet core platform need instant, accurate answers to CLI configuration, operational, and troubleshooting questions — grounded in validated CLI inventory, product PDF documentation, and tech-support outputs. The critical constraint: CLI commands must be rendered with **zero hallucination**, because an invented flag can break a live network node.
- **Architecture Pattern:** Full-stack RAG driven by an intent-classification pipeline (intent → retrieval → command validation → deterministic rendering → generation) over a unified hybrid search index, fronted by a global edge with WAF and running on a fully private-networked Azure backbone. The LLM explains and frames; retrieved commands are validated against a CLI inventory and rendered deterministically.
- **Azure Services:**

  | Service | Role |
  |---------|------|
  | Azure Front Door + WAF | Global edge routing, DDoS protection, WAF security policies |
  | Azure Container Apps | FastAPI (Python 3.11) backend host |
  | Azure Container Registry | Backend container images |
  | Static web frontend | Vite + TypeScript SPA |
  | Azure OpenAI (frontier GPT model) | Response generation and intent classification |
  | Azure OpenAI (text-embedding-3-large) | Embeddings for vector search |
  | Azure AI Search | Unified hybrid index (vector + keyword) across all source types |
  | Azure Cosmos DB | Conversations, evaluation results, and audit logs |
  | Azure Blob Storage | Source documents / ingestion staging |
  | Azure Functions | Offline evaluation pipeline (retrieval quality, chunk coverage) |
  | Azure AI Foundry (Hub + Project) | Model governance and experiment tracking |
  | Microsoft Entra ID | JWT authentication, managed identity |
  | Azure Key Vault | Secrets (e.g. JWT signing key) |
  | Private Endpoints + VNet | Private networking for OpenAI, Search, Cosmos DB, Key Vault, Blob, ACR, Functions |
  | App Insights + Log Analytics | Observability with diagnostics on every resource |

- **Key Design Decisions:**

  | Decision | Rationale |
  |----------|-----------|
  | Deterministic CLI rendering | Intent → retrieval → validation → rendering pipeline eliminates hallucinated CLI commands |
  | Managed identity + RBAC everywhere | Zero stored credentials; scoped role assignments per service |
  | Unified hybrid index | A single AI Search index (vector + keyword) spans CLI specs, PDFs and tech-support outputs |
  | Offline ingestion pipeline | CLI-tree/YANG, PDF and tech-support chunkers + indexers build the index out-of-band |
  | Private networking by default | Private endpoints + VNet integration for every data and AI service |
  | Edge WAF | Azure Front Door security policies enforce protection at the perimeter |
  | Built-in evaluation | A Functions pipeline scores retrieval quality and chunk coverage; results stored in Cosmos DB |
  | Safety by design | Red-team test suite, content-safety guardrails, PII redaction, and a dedicated audit log |

- **Scope:** Architecture lead / design authority for the initiative — designed for production, with governance, STRIDE threat modelling and Responsible AI review built into the design. Authored the Architecture Decision Record set (platform selection, authN/authZ, multi-tenancy, Responsible AI, threat model, open-source governance, cost).
- **Trade-off:** *Why deterministic validation over pure LLM generation?* For CLI commands a hallucinated flag can break a production network node, so retrieved commands are validated against a CLI inventory and rendered deterministically — the LLM is used only for explanation and framing.
- **Value Proposition:** Turns hours of manual documentation and CLI lookup into instant, citation-backed answers with guaranteed-valid CLI commands — private-networked, observable, and governed for Responsible AI. Designed for global scale, it targets a substantial reduction in formal support tickets — answering directly or helping engineers raise better-framed tickets that resolve faster.

- **Architecture Diagram:**

<div class="mermaid">
flowchart LR
    ENG["Support Engineer"] --> ASK["Natural-language<br/>question"]
    ASK --> RET["Retrieve<br/>(hybrid search over<br/>validated docs)"]
    RET --> VAL["Validate<br/>(commands checked<br/>against inventory)"]
    VAL --> GEN["Generate<br/>(grounded explanation)"]
    GEN --> OUT["Cited answer with<br/>verified CLI commands"]
    OUT --> ENG

    subgraph FND["Cross-cutting foundations"]
        direction LR
        SEC["Private networking ·<br/>Managed identity"]
        GOV["Responsible AI ·<br/>Evaluation · Red-team"]
    end
    RET -.-> FND
    VAL -.-> FND

    style VAL fill:#F0F8E8,stroke:#7BC67E,stroke-width:2px
</div>

*High-level capability view: a support engineer's question is answered by retrieving from validated documentation, checking any CLI commands against a known inventory, and only then generating a grounded, cited response — with security and Responsible-AI foundations underneath.*

## 4. Skills + MCP Triage Assistant with a Supervised Agent {#work-triage-assistant}
- **Scenario:** A telco support team triaging thousands of network tickets needs faster, more consistent triage and deep analysis — without a bespoke platform-bound tool and without removing the engineer from the decision loop. An earlier ticketing-platform-native, multi-agent design coupled the solution to a single platform and introduced a central orchestrator as a single point of failure; the architecture was pivoted to a composable model.
- **Architecture Pattern:** Two clean abstractions — **Skills** (procedural knowledge; one skill = one task) that compose **MCPs** (Model Context Protocol adapters; one MCP = one system) — invoked by an engineer, a scheduler, or a **supervised triage agent** on an internal agent runtime. The agent watches the queue, composes a default skill chain per ticket, and posts a structured summary to the team chat; the engineer supervises and applies every external change natively. No central orchestrator, no platform lock-in.
- **Composition Model:**

  | Building block | Role |
  |----------------|------|
  | Skills (Microsoft SKILL.md format) | Validation, Enrichment, Deep Analysis (10-step drill), Next Steps Proposal, Queue Sweep, quality-check and handoff-summary skills |
  | MCPs (reuse in-house first) | Ticketing, knowledge base, incident management, engineering + escalation trackers, domain troubleshooting, team chat |
  | Invokers | Support engineer (chat/thread), scheduler, supervised triage agent — Phase 2 adds an autonomous runtime |
  | Supervised agent loop | Watch queue → compose skill chain → post summary to chat → engineer supervises |
  | Deep-analysis drill | 10-step methodology: ticket + conversations → similar cases → incident correlation → domain guidance → engineering bugs → escalation status → version-gap → draft response → resolution pattern → synthesis |

- **Key Design Decisions:**

  | Decision | Rationale |
  |----------|-----------|
  | Decouple from the ticketing platform | The ticketing system becomes one MCP among many — no custom plugins, no platform lock-in |
  | Two clean abstractions | Skills hold procedural knowledge; MCPs hold system access — interpretation never leaks into adapters |
  | Supervised now, autonomous later | Phase 1 is human-supervised on an internal agent runtime; Phase 2 swaps in an autonomous runtime — an invoker change, not a rewrite |
  | No central orchestrator | The agent composes via the catalogue; one skill or MCP failing degrades only that branch (no single point of failure) |
  | Reuse over rebuild | Existing in-house MCPs plug in unchanged; a new MCP is built only when nothing in-house fits |
  | Human approves external writes | Phase 1 has exactly one write — the summary post; all ticket, incident and customer-comms changes are applied by the engineer |
  | Audit everything | Every agent, skill, and MCP call is logged with caller identity and outcome |

- **Scope:** Architecture lead / design authority — set the business case and MVP scope, authored the Architecture Decision Records (including the pivot decision that superseded the original design), and defined the guardrails plus the **architecture-governance framework** used to check the implementation against the design for security, Responsible AI, privacy, and threat-model review.
- **Trade-off:** *Why abandon a working platform-native design?* Platform coupling locked the solution's value into one ticketing tool and concentrated failure in its plugins. Re-basing on Skills + MCP trades some short-term rework for reuse across surfaces, no single point of failure, and a Phase-2 path to autonomy that is an invoker swap rather than a re-platform.
- **Value Proposition:** Turns queue-hunting into a co-worker that analyses every new ticket and summarises it for the engineer — composable, platform-agnostic, human-supervised, and built to graduate to autonomy without redesign. It is designed to give engineers a single interface across many systems and to cut triage and troubleshooting from hours to minutes, pairing generative AI with deterministic automation for consistent quality.

- **Architecture Diagram:**

<div class="mermaid">
flowchart LR
    subgraph INV["Invokers"]
        direction TB
        I1["Engineer"]
        I2["Scheduler"]
        I3["Supervised Agent"]
        I4["Autonomous runtime<br/>(Phase 2)"]
    end
    INV --> SK["Skills<br/>(procedural knowledge —<br/>one task each)"]
    SK --> MCP["MCPs<br/>(system access —<br/>one system each)"]
    MCP --> SYS["Enterprise systems<br/>(ticketing, KB, incidents,<br/>trackers, chat)"]
    I3 -.->|"watch queue →<br/>summarise to chat"| HUM["Engineer supervises ·<br/>approves all external writes"]

    style INV fill:#E8F4FD,stroke:#4A90D9,stroke-width:2px
    style SK fill:#F0F8E8,stroke:#7BC67E,stroke-width:2px
    style MCP fill:#FFF4E0,stroke:#E89B3C,stroke-width:2px
    style I4 stroke-dasharray: 5 5
</div>

*High-level capability view: task-specific skills compose system-access MCPs, invoked by an engineer, a scheduler, or a supervised agent that watches the queue and summarises to chat — while the engineer approves every external change. A future autonomous runtime reuses the same skills and MCPs.*

---

[← Back to Portfolio](../)
