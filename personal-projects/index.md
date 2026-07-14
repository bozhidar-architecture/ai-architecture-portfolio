---
title: Personal Projects & Labs
layout: single
toc: true
toc_sticky: true
toc_label: "Projects"
description: Personal AI and infrastructure labs — an always-on cloud transcription pipeline and an infrastructure-as-code edge network blueprint, built with carrier-grade discipline.
---

{% include mermaid-lightbox.html %}

*Beyond client work, I build for myself — the same principles, no deadline. It's where "AI-first" and "carrier-grade discipline" stop being slogans and become habits.*

## Always-On Live Transcription Pipeline {#work-transcription}
- **What it is:** an always-on personal service that continuously monitors **publicly available** live streams and produces searchable, **speaker-attributed** transcripts — fully unattended, 24/7. It operates only on public streams; no private audio is processed.
- **Architecture:** a timer-driven serverless watcher (Azure Functions) detects active streams and spins up **ephemeral compute (Azure Container Instances) per stream** to capture audio segments to storage; a **blob trigger** then transcribes each segment via a **managed cloud speech-to-text service with speaker diarization**, with an LLM for Q&A and summaries. Streams are isolated, the whole stack is infrastructure-as-code (Bicep) deployed via `azd`, and it self-heals — auto-reconnect on drops, retry-with-backoff, and a timer that re-runs any missed transcriptions.
- **Beyond building it:**
  - **Obsolescence-proofing** — proactively migrated off a **retiring serverless hosting plan** to its successor with a **make-before-break** cutover: the full stack cloned into a second environment, old and new running **in parallel** with zero shared state, then a clean switch-over — no downtime.
  - **Cost analysis & optimisation** — modelled pricing across hosting plans and tuned for a **scale-to-zero, pay-per-use** profile (~€1.40 for a full multi-hour session), with budget alerts as a guardrail.
- **Why it matters:** genuine end-to-end lifecycle ownership — build → operate → migrate → optimise — on a resilient, low-cost, unattended design.

<div class="mermaid">
flowchart LR
    SRC["Public live streams<br/>(monitored 24/7)"] --> TIMER["Timer-driven watcher<br/>(Azure Functions)"]
    TIMER --> ACI["Ephemeral compute per stream<br/>(Azure Container Instances)"]
    ACI --> BLOB["Audio segments<br/>(Blob Storage)"]
    BLOB -->|blob trigger| STT["Speech-to-Text<br/>+ speaker diarization"]
    STT --> STORE["Searchable, speaker-attributed<br/>transcripts — retained for personal<br/>research use, reviewed periodically"]
    STORE --> LLM["LLM Q&A<br/>+ summaries"]

    subgraph RES["Self-healing"]
        direction TB
        R1["Auto-reconnect on drops"]
        R2["Retry with backoff"]
        R3["Timer re-runs missed jobs"]
    end
    ACI -.-> RES
    STT -.-> RES

    style TIMER fill:#E8F4FD,stroke:#4A90D9,stroke-width:2px
    style STORE fill:#F0F8E8,stroke:#7BC67E,stroke-width:2px
    style RES fill:#FFF4E0,stroke:#E89B3C,stroke-width:2px
</div>

*Runs entirely in the cloud, forever and unattended — no local client involved. A serverless watcher spins up isolated, ephemeral compute per stream; each audio segment is transcribed on a blob trigger and stored as a searchable, speaker-attributed transcript, with an LLM layered on top for Q&A and summaries. Scale-to-zero when idle, self-healing when things drop.*

## Infrastructure-as-Code Edge Network — an AI-Assisted Delivery Blueprint {#work-edge-network}
- A network gateway run **entirely as code** — and a **reference blueprint** for **AI-first** infrastructure delivery in telecom, network, and delivery organisations. The Git repo is the **single source of truth**: every change is **version-controlled, staged, snapshot-backed, and reversible with one command**, drafted with AI assistance but **reviewed and verified by a human**, with secrets kept out of the repo and a full audit trail.
- Carrier-grade change discipline (baseline → staged apply → verify → guaranteed rollback), made **AI-native** — a template an organisation could adopt to move fast *and* stay safe, auditable, and reversible.
- **Security posture — the repo holds metadata, never secrets:** the source of truth is a machine-readable model of *intent* (network names, VLANs, roles, topology) — real passwords and Wi-Fi keys never enter it. Secrets are injected only at render time from a git-ignored store, or set on the device directly. Anything that *is* versioned is **redacted by default**; full backups (which carry credentials and one-way password hashes) are kept off-repo.
- **Encrypted at rest, least-exposure by design:** the design and any secret-bearing files can be sealed with **passphrase-only encryption (no key stored on disk)**, so the real configuration lives on disk only as ciphertext until it's deliberately opened to work on. On the network side it's least-exposure — a default-deny firewall, segmented VLANs, and a management plane reachable only from an isolated admin network.

<div class="mermaid">
flowchart LR
    subgraph AUTHOR["1 · Design & intent — Git is the source of truth"]
        direction TB
        HLD["High-level design<br/>(intent, zones, topology)"] --> CIQ["Intent data model<br/>(metadata only — no secrets)"]
        AI["AI-assisted<br/>authoring"] -.-> CIQ
        REV["Human review<br/>+ approval"] -.-> CIQ
    end

    subgraph SECRETS["Secrets — kept separate"]
        direction TB
        VAULT["Vault<br/>(encrypted at rest;<br/>real secrets vault in production)"]
    end

    CIQ --> RENDER["2 · Render<br/>(model + template → device config)"]
    VAULT -. injected at render .-> RENDER
    RENDER --> STAGE["3 · Stage<br/>(next config — versioned)"]

    subgraph CHANGE["4 · Controlled change — every change is a MOP"]
        direction TB
        BASE["Baseline backup<br/>+ health capture"] --> APPLY["Apply to device"]
        APPLY --> VERIFY["Verify<br/>(post-change health)"]
        VERIFY -->|pass| SNAP["Snapshot<br/>(running → history)"]
        VERIFY -->|fail| ROLL["Rollback<br/>(one command)"]
    end

    STAGE --> CHANGE
    CHANGE --> DEV["Network device(s)"]

    DEV -. scheduled export .-> DRIFT["5 · Drift detection<br/>(live vs intent)"]
    DRIFT -. divergence → fix in intent .-> CIQ

    style CIQ fill:#E8F4FD,stroke:#4A90D9,stroke-width:2px
    style VAULT fill:#FFF4E0,stroke:#E89B3C,stroke-width:2px
    style SNAP fill:#F0F8E8,stroke:#7BC67E,stroke-width:2px
    style ROLL fill:#FCE8E8,stroke:#D9534F,stroke-width:2px
</div>

*The end-to-end operating model: intent is authored as a versioned data model (AI-assisted, human-approved), secrets stay in a separate vault and are injected only at render time, and the generated config is staged and applied under a Method-of-Procedure — baseline → apply → verify, then snapshot on success or one-command rollback on failure. A scheduled drift check compares the live device against intent and feeds any divergence back into the model. It's how I run my own home network — but the same loop operates **any** network as code (in production, the vault is a managed secrets service).*

---

[← Back to Portfolio](../)
