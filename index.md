---
title: "AI Architecture Portfolio"
layout: single
author_profile: true
toc: true
toc_label: "On this page"
toc_sticky: true
description: AI solution architecture with carrier-grade discipline — agentic AI, RAG, multi-agent systems, and governance frameworks for enterprise telco. 20+ years of mission-critical infrastructure applied to AI.
---

{% include mermaid-lightbox.html %}

Welcome to my **AI Architecture Portfolio**, showcasing solution designs, governance frameworks, and business-to-technical mappings for **AI-driven enterprise and telco use cases**.

This portfolio demonstrates **architecture thinking**, **Responsible AI principles**, and **integration strategies** — depth of design over implementation detail. (The hands-on builds live in [Personal Projects & Labs](personal-projects/).)

> Carrier-grade discipline applied to AI: cloud-native where it fits, **sovereign and air-gapped where it's required**. Twenty years of mission-critical telco infrastructure, now aimed at the systems the cloud can't reach.

## Selected Work

- **[Software Release Impact Advisor](reference-architectures/#work-release-advisor)** — a declarative RAG agent that replaced hours of manual upgrade analysis and scaled across products
- **[Privacy-First Configuration Analyzer](reference-architectures/#work-config-analyzer)** — an offline, zero-telemetry tool adopted for licensing true-ups
- **[RAG Assistant for Support Engineering](reference-architectures/#work-rag-assistant)** — guaranteed-valid CLI commands with LLM-framed, citation-backed answers on a private-networked Azure backbone
- **[Skills + MCP Triage Assistant](reference-architectures/#work-triage-assistant)** — a composable, human-supervised agent built to graduate to autonomy

> *All architectures presented here are original reference designs inspired by real-world implementations in enterprise telco environments. They reflect general industry patterns and publicly available cloud services.*
>
> *These designs are deliberately anonymised — I'm happy to go deeper in a conversation. [Reach out on LinkedIn →](https://www.linkedin.com/in/bozhidar/)*

### Portfolio at a Glance

<!-- Maintenance note: this map groups work by *deployment spectrum*, not by exact project list.
     Add/rename a node only when a whole new project appears — day-to-day edits shouldn't touch it. -->

<div class="mermaid">
flowchart LR
    subgraph CN["Cloud-native"]
        direction TB
        RAG["RAG Support Assistant"]
        TRIAGE["Skills + MCP Triage"]
        TRANS["Live Transcription Pipeline"]
        RELEASE["Release Impact Advisor"]
    end
    subgraph EDGE["Edge / hybrid"]
        direction TB
        IAC["IaC Edge Network"]
    end
    subgraph SOV["Sovereign / air-gapped"]
        direction TB
        CONFIG["Config Analyzer (offline)"]
        LAB["Air-gapped inference lab<br/>(where I'm headed)"]
    end
    CN --> EDGE --> SOV

    style CN fill:#E8F4FD,stroke:#4A90D9,stroke-width:2px
    style EDGE fill:#FFF4E0,stroke:#E89B3C,stroke-width:2px
    style SOV fill:#F0F8E8,stroke:#7BC67E,stroke-width:2px
    style LAB stroke-dasharray: 5 5
</div>

*One view of the whole portfolio along a single axis — from cloud-native delivery today toward sovereign, air-gapped AI. The direction of travel is deliberate.*

---

## About Me

- **Role:** Principal Architect
- **Background:** 20+ years in telco mobile packet core, carrier-grade infrastructure, and enterprise architecture
- **Focus:** Agentic AI design, declarative agent architectures, AI-assisted development workflows, and the enablement to make teams AI-first
- **Mindset:** Every solution is designed with mission-critical principles — because AI systems serving telecom operators inherit the same availability and resilience expectations as the networks they support.
- **Connect:** [LinkedIn →](https://www.linkedin.com/in/bozhidar/)

---

## Where I'm Headed — Sovereign & Air-Gapped AI

My next focus builds directly on two decades in private networks, private cloud, and critical infrastructure: **AI that runs where the cloud can't reach — cloud-native where it fits, fully sovereign where it's required.**

- **Sovereign & air-gapped by design** — many regulated sectors and nations will never allow their data to leave national borders or enter a public cloud. I treat that not as a limitation to work around, but as a first-class design target: defence, government, telecom core, and critical national infrastructure need AI that runs fully on-premises or entirely disconnected.
- **Local, open-weight models** — as open-weight models converge with frontier capability, running capable inference on-premises — even on a single air-gapped workstation — becomes practical for real workloads, not just demos.
- **The differentiator is the ecosystem, not the model** — once raw model capability commoditises, durable value shifts to everything around it: retrieval and data pipelines, orchestration, governance and Responsible AI, security, integration with legacy systems, and trusted delivery and operations. Whoever can *deliver and operate* the whole system — safely, under real-world constraints — wins.
- **Grounded in practice** — I've already shipped offline, air-gapped tooling ([see the Configuration Analyzer](reference-architectures/#work-config-analyzer)), and I'm building a personal air-gapped inference lab to keep this honest about what actually works on real hardware, disconnected.

This is a thesis, not a certainty — but it's where I'm placing my bets: **carrier-grade, sovereign, ecosystem-first AI.**

---

## Architecture Principles

These principles come from two decades of building and operating carrier-grade telco systems, now applied to AI solution design:

| Principle | What It Means In Practice |
|-----------|--------------------------|
| **Five-nines thinking** | Design for 99.999% availability — graceful degradation, no single points of failure |
| **Zero Trust security** | Managed identity, no API keys, least-privilege access, encrypt everything in transit and at rest |
| **Disaster recovery by design** | Multi-region readiness, data replication, recovery objectives defined upfront — not bolted on later |
| **Modular & composable** | Independent components that can be replaced, scaled, or retired without cascading impact |
| **Operationally simple** | If it's hard to operate, it's badly designed. Observability, structured logging, and clear runbooks from day one |
| **Built to be maintained** | Architecture decisions documented (ADRs), infrastructure as code, automated drift detection |
| **Resilient by default** | Retry policies, circuit breakers, graceful fallbacks — assume failure will happen |
| **Compliance-native** | Responsible AI, accessibility, privacy, and license compliance integrated from the start — not an afterthought |

---

## [Governance & Compliance →](governance-compliance/)

## [Reference Architectures →](reference-architectures/)

## [Personal Projects & Labs →](personal-projects/)

---

## Tools & Frameworks

- **AI & Agentic Platforms:** Azure AI Foundry, Microsoft Copilot Studio, OpenAI, RAG patterns, semantic re-ranking
- **AI-Assisted Development:** VS Code, GitHub Copilot, MCP (Model Context Protocol) integrations, prompt engineering
- **Architecture Design:** Lucidchart, Draw.io, Mermaid
- **DevOps & Collaboration:** Azure DevOps, Git, CI/CD pipelines
- **Governance:** Responsible AI principles, threat modelling, privacy & license compliance
- **Portfolio Hosting:** GitHub Pages

---

## AI Certifications

*AI-specific credentials only — non-AI certifications are not listed here.*

- ✅ [Microsoft Certified: Agentic AI Business Solutions Architect (AB-100)](https://learn.microsoft.com/en-us/credentials/certifications/agentic-ai-business-solutions-architect/)
- ✅ [Microsoft Certified: Azure AI Engineer Associate (AI-102)](https://learn.microsoft.com/en-us/credentials/certifications/azure-ai-engineer/)
- ✅ [Microsoft Certified: Azure AI Fundamentals (AI-900)](https://learn.microsoft.com/en-us/credentials/certifications/azure-ai-fundamentals/)

<!-- TODO (owner): these link to the generic Microsoft Learn certification pages. Swap in your personal transcript/verification links if you'd rather link those instead. -->

### Continuing Development
- 🔄 Working through **Anthropic Academy's Claude courses** toward the **Claude Certified Architect — Foundations (CCA-F)** certification, to broaden multi-vendor agentic-architecture fluency on top of AB-100

---

## AI Enablement & Organisational Impact

Beyond building solutions, a core part of my impact is **enabling others** — showing engineers with no coding background what they can achieve with AI tools and modern development practices. I believe the clearest divide over the next decade will be between organisations that adopt an **AI-first mindset** and those that treat AI as a bolt-on: the former compound their advantage, the latter fall behind.

<div class="mermaid">
flowchart LR
    S1["AI as a bolt-on<br/>(a minority of engineers)"] --> S2["AI-assisted development<br/>(adopted by the majority)"]
    S2 --> S3["Domain experts ship<br/>their own AI solutions"]
    S3 --> S4["AI-first operating model<br/>'design as if AI is assumed'"]

    subgraph LEVERS["The levers I drove"]
        direction TB
        L1["Art-of-the-possible<br/>showcases"]
        L2["Copilot + VS Code<br/>adoption & coaching"]
        L3["Intent-based development<br/>(spec as source of truth)"]
        L4["Modular AI-cert<br/>pathways"]
    end
    LEVERS -.-> S2
    LEVERS -.-> S3

    style S4 fill:#F0F8E8,stroke:#7BC67E,stroke-width:2px
    style LEVERS fill:#E8F4FD,stroke:#4A90D9,stroke-width:2px
</div>

*The shift I've led inside a large, mission-critical organisation — from AI as a side project to an AI-first operating model — and the enablement levers that move teams along it.*

### What I've Driven
- **Demonstrated the art of the possible** — hands-on showcases proving that domain experts (not just developers) can build production-grade AI solutions using LLMs and low-code/no-code tooling
- **Intent-based development adoption** — evangelised spec-as-source-of-truth workflows where the specification drives development, not the other way around
- **AI-assisted development culture** — drove GitHub Copilot and VS Code adoption across a large telco delivery organisation, from a minority of engineers to the majority, and coached domain experts with no coding background to ship their own working applications
- **AI-first operating model** — moved teams from "can AI help with this task?" to "how would we design this if AI were assumed?" — reshaping workflows, not just tooling
- **Certification pathways** — designed modular training paths for Azure AI certifications, enabling teams to upskill progressively
- **Lifelong skills focus** — positioned AI fluency not as a project skill, but as a career-defining capability that compounds over time

### Why This Matters
AI-first organisations will outcompete those that treat AI as a side project. The advantage doesn't come from one flagship model — it comes when everyday domain experts can build and improve with AI themselves. I've led that shift inside a large, mission-critical organisation, and it's the capability I bring to any team that wants a durable, compounding AI advantage.

*The same tools behind the reference architectures above are what I teach teams to wield.*

---

*Let's connect — [LinkedIn →](https://www.linkedin.com/in/bozhidar/)*
