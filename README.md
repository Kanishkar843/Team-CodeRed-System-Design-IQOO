<div align="center">

# 💖 SoulSync — System Design Specification
### *Mobile-First, 100% On-Device AI Dating & Social Connection Platform*
**Engineered for iQOO 15 (Qualcomm Snapdragon 8 Elite Gen 5 NPU)**

[![Platform](https://img.shields.io/badge/Platform-Android_Kotlin_%2B_Compose-3DDC84?style=for-the-badge&logo=android&logoColor=white)](#)
[![AI Hardware](https://img.shields.io/badge/NPU-Qualcomm_Snapdragon_8_Elite-EE3124?style=for-the-badge&logo=qualcomm&logoColor=white)](#)
[![AI Models](https://img.shields.io/badge/On--Device_LLM-Gemma_3_4B_int4-4285F4?style=for-the-badge&logo=google&logoColor=white)](#)
[![Privacy](https://img.shields.io/badge/Privacy-100%25_Local_Inference-47223B?style=for-the-badge)](#)
[![Compliance](https://img.shields.io/badge/Compliance-DPDP_Act_2023_(India)-C9A27E?style=for-the-badge)](#)

---

</div>

## 🌟 Brand Visual Identity & Color Tokens

SoulSync diagrams and user interface components utilize a signature harmonious palette:

| Token Name | Hex Code | Visual Sample | Architectural Usage |
| :--- | :--- | :---: | :--- |
| **Ivory Canvas** | `#FFF9F7` | `████████` | Primary background, clean canvas backdrop for visual diagrams. |
| **Blush Pink** | `#F9E4EA` | `████████` | Presentation layer UI, client sandboxes, and local data containers. |
| **Rose Accent** | `#E79BAF` | `████████` | On-device AI execution core, NPU pipeline paths, and active flows. |
| **Deep Plum** | `#47223B` | `████████` | Primary system boundaries, headers, text, and security interceptors. |
| **Soft Gold** | `#C9A27E` | `████████` | "101 Cosmic Match" rarity badges, cryptographic keys, and offline mesh. |

---

## 📄 Master System Design Specification Index

All primary system design documents are located under [`docs/system-design/`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/):

| Document File | Topic Title | Detailed Summary (What the File Contains) |
| :--- | :--- | :--- |
| 📑 [`00-overview.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/00-overview.md) | **Product Vision & Scope** | Defines the core problem statement (dating app fatigue & privacy leaks), hardware specs for iQOO 15 (Snapdragon 8 Elite Gen 5 NPU), the 5-phase user lifecycle, and explicit architectural non-goals. |
| 🏛️ [`01-high-level-architecture.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/01-high-level-architecture.md) | **High-Level Design (HLD)** | Provides the complete 5-layer system architecture (Presentation, Storage, NPU AI Core, Connectivity Mesh, Cloud Relay) with component rationale (WHAT, WHY, WHAT BREAKS WITHOUT IT). |
| 🔄 [`02-low-level-design.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/02-low-level-design.md) | **Low-Level Design (LLD)** | Details step-by-step sequence diagrams for 5 critical user flows: Registration, Phase 1 AI Chat, Phase 2 Vector Extraction, Phase 3 Cosmic Matching, and Phase 4 PII Guardrails. |
| 🗄️ [`03-data-model.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/03-data-model.md) | **Data Model & Schemas** | Specifies local Encrypted Room SQLite entity tables (`User`, `Profile`, `PersonalityVector`, `Match`, `ChatMessage`, `GuardrailEvent`), TTL purge policies, and SQLCipher AES-256 KeyStore encryption. |
| 📡 [`04-api-spec.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/04-api-spec.md) | **API Specifications** | Documents REST endpoints for Firebase Auth exchange, vector submission, candidate fetching, WebSocket real-time chat streams, FCM push triggers, and Nearby Connections IPC payloads. |
| 🧠 [`05-on-device-ai.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/05-on-device-ai.md) | **On-Device AI Engine** | Explores Gemma 3 (4B int4) and EmbeddingGemma 300M (128-dim MRL) memory ergonomics, NPU context budgeting (3,584 tokens), rolling summarization, and 3-tier PII safety guardrails. |
| ⚖️ [`07-scaling-and-tradeoffs.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/07-scaling-and-tradeoffs.md) | **Scaling & Trade-Offs** | Analyzes global model CDN differential updates, regional LSH match relays, NPU thermal/power duty cycle budgeting (<1.2W draw), and architectural alternatives considered vs. rejected. |
| 🛡️ [`08-risk-safety-ethics.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/08-risk-safety-ethics.md) | **Risk, Safety & Ethics** | Details 18+ identity age-gating, DPDP Act 2023 (India) legal alignment, local abuse reporting, crisis hotline intervention (Tele-MANAS `14416` & iCall), and honest guardrail boundaries. |

---

## 🎨 Master Visual Diagrams Breakdown

All visual diagrams are authored in pure Mermaid syntax under [`docs/system-design/diagrams/`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/diagrams/):

| Diagram File | Diagram Type | Detailed Content & Purpose (What the Diagram Explains) |
| :--- | :--- | :--- |
| 📐 [`architecture-overview.mmd`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/diagrams/architecture-overview.mmd) | **C4 Container System Architecture** | Visualizes the complete boundary separation between the local iQOO Android device (Compose UI, SQLCipher DB, Snapdragon NPU AI Core, Nearby Mesh) and the minimal cloud relay (Firebase Auth, Match Relay, FCM Push). |
| 🔄 [`on-device-ai-flow.mmd`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/diagrams/on-device-ai-flow.mmd) | **On-Device AI Sequence Flow** | Illustrates the step-by-step processing of user text through Gemma 3 NPU inference (~28 tok/s), EmbeddingGemma 128-dim MRL vector extraction, local storage, and Tier 1 Regex PII guardrail blocking. |
| 📱 [`tap-to-connect-flow.mmd`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/diagrams/tap-to-connect-flow.mmd) | **Tap-to-Connect P2P Flow** | Maps the offline device-to-device interaction flow: BLE advertisement beacon scanning, Wi-Fi Direct P2P handshake with ECDH key exchange, offline 128-dim vector swap, and direct encrypted chat bootstrap. |
| 🎯 [`matching-flow.mmd`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/diagrams/matching-flow.mmd) | **Vector Scoring & Cosmic 101 Rarity Logic** | Flowchart detailing how candidate vectors undergo cosine similarity calculation, score normalization $\text{score} = \text{round}\left(\frac{\text{cos} + 1}{2} \times 100\right)$, top 0.5% threshold check, rare niche tag overlap ($\ge 2$), and "101 Cosmic Match" badge award logic. |
| 🛠️ [`mermaid-rendering-instructions.md`](file:///c:/Users/Administrator/Downloads/Team-CodeRed-System-Design-IQOO/docs/system-design/diagrams/mermaid-rendering-instructions.md) | **CLI Render Guide** | Provides exact `npx @mermaid-js/mermaid-cli` commands, background color flags (`#FFF9F7`), and batch conversion scripts for rendering all `.mmd` diagrams to high-resolution PNG or SVG images. |

---

## 🛠️ Quick Diagram Render Guide

To render any of the `.mmd` files into PNG images with the brand background (`#FFF9F7`), execute:

```bash
npx -p @mermaid-js/mermaid-cli mmdc -i docs/system-design/diagrams/architecture-overview.mmd -o docs/system-design/diagrams/architecture-overview.png --background "#FFF9F7"
```

---

<div align="center">

**SoulSync Architecture** • Engineered for Privacy, Performance & Human Connection  
*Team CodeRed — iQOO Hackathon 2026*

</div>