# SoulSync — System Design Specification Index

> [!NOTE]
> **TL;DR**  
> **Who cares:** Hackathon evaluators, mobile software architects, privacy advocates, and NPU performance engineers.  
> **What it does:** Serves as the master index for SoulSync’s complete, publication-quality system design architecture.  
> **Why this approach:** Provides structured, modular documentation covering high-level architecture, on-device AI pipelines, low-level sequences, data models, APIs, and safety protocols.  
> **What it costs:** 5-minute total reading time; zero server-side AI compute overhead.

---

## Executive Summary

**SoulSync** is a mobile-first, privacy-preserving dating and social connection platform designed for Android (Kotlin + Jetpack Compose) targeting the **iQOO 15** (Snapdragon 8 Elite Gen 5 NPU) with fallback support for Snapdragon 8 Gen 2+ devices. 

Unlike traditional dating applications that collect personal chats on centralized servers and promote superficial swiping, SoulSync executes **100% of its AI logic locally on the device NPU** (Neural Processing Unit). By pairing Qualcomm AI Engine Direct (QNN) and LiteRT with **Gemma 3 (4B QAT int4)** and **EmbeddingGemma 300M (128-dim MRL)**, SoulSync analyzes user personality traits and enforces safety guardrails without a single byte of chat data leaving the smartphone.

---

## Documentation Tree

All system design documents are located under `docs/system-design/`:

| File Path | Document Title | Target Audience | Key Topic Covered |
| :--- | :--- | :--- | :--- |
| [`00-overview.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/00-overview.md) | **Product Overview & Vision** | Evaluators, Product Leads | Problem statement, 5-phase user lifecycle, hardware targets, non-goals. |
| [`01-high-level-architecture.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/01-high-level-architecture.md) | **High-Level Architecture (HLD)** | System Architects | 4-tier layer design, ASCII architecture diagram, component rationale. |
| [`02-low-level-design.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/02-low-level-design.md) | **Low-Level Design (LLD)** | Lead Developers | ASCII sequence diagrams for 5 critical user flows & PII guardrails. |
| [`03-data-model.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/03-data-model.md) | **Data Model & Schemas** | Database Engineers | Local Room SQLite schemas, EncryptedSharedPreferences, TTLs, and cloud relay payloads. |
| [`04-api-spec.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/04-api-spec.md) | **API Specifications** | Backend & Mobile Integrators | Firebase Auth REST APIs, Minimal Relay WebSocket endpoints, Nearby IPC formats. |
| [`05-on-device-ai.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/05-on-device-ai.md) | **On-Device AI Engine** | AI/ML Engineers | Gemma 3 int4 ergonomics, EmbeddingGemma 128-dim MRL, prompt budget, PII guardrails. |
| [`06-mvp-30-hour-plan.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/06-mvp-30-hour-plan.md) | **30-Hour Hackathon Plan** | Sprint Managers | Hour-by-hour timeline, track division, Real vs. Mocked implementation matrix. |
| [`07-scaling-and-tradeoffs.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/07-scaling-and-tradeoffs.md) | **Scaling & Architectural Trade-Offs** | Infrastructure Engineers | Model CDN strategy, NPU thermal/power budgeting, rejected alternatives. |
| [`08-risk-safety-ethics.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/08-risk-safety-ethics.md) | **Risk, Safety & Ethics** | Compliance & Safety Leads | 18+ gating, DPDP Act (India) alignment, crisis hotline integration, guardrail limits. |
| [`diagrams/`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/diagrams/) | **Mermaid Diagram Source Files** | System Designers | Master `.mmd` files and CLI rendering instructions. |

---

## Brand Visual Identity & Color Palette

System design diagrams maintain a consistent color palette tailored for the SoulSync brand:

* **Ivory Canvas (`#FFF9F7`):** Primary diagram background.
* **Blush Pink (`#F9E4EA`):** Secondary container highlights and local storage components.
* **Rose (`#E79BAF`):** Accent lines, flow indicators, and active execution paths.
* **Deep Plum (`#47223B`):** System boundaries, primary text, and core engine blocks.
* **Soft Gold (`#C9A27E`):** "101 Cosmic Match" rarity badges and cryptographic keys.

---

## Quick Start — Rendering Visual Diagrams

All diagrams are authored in pure Mermaid syntax (`.mmd`). To render them into high-resolution PNG/SVG images, refer to [`diagrams/mermaid-rendering-instructions.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/diagrams/mermaid-rendering-instructions.md) or run:

```bash
npx -p @mermaid-js/mermaid-cli mmdc -i docs/system-design/diagrams/architecture-overview.mmd -o docs/system-design/diagrams/architecture-overview.png --background "#FFF9F7"
```
