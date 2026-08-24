# 06 — MVP 30-Hour Hackathon Plan

> [!NOTE]
> **TL;DR**  
> **Who cares:** Hackathon judges, sprint managers, demo leads, and software developers.  
> **What it does:** Outlines an hour-by-hour 30-hour hackathon execution roadmap distinguishing production code from pitch mocks.  
> **Why this approach:** Maximizes working demo impact on iQOO 15 hardware while prioritizing core NPU integration and Tap-to-Connect flows.  
> **What it costs:** 30 team-hours divided across 3 parallel developer tracks.

---

## Acronym Glossary

* **MVP (Minimum Viable Product):** A product version with just enough features to be usable by early customers and evaluated.
* **NPU (Neural Processing Unit):** On-device silicon processor for AI inference.
* **BLE (Bluetooth Low Energy):** Wireless short-range communication protocol.
* **UI/UX (User Interface / User Experience):** Design and interactive flow of the application.
* **PR (Pull Request):** Proposed code change submitted to a version control repository.

---

## Team Track Division (3 Parallel Streams)

* **Track A (Android UI/UX Lead):** Onboarding flows, Jetpack Compose layouts, Hinge-style profile UI, Phase 4 anonymous chat views, screenshot protection.
* **Track B (AI Engine & NPU Lead):** LiteRT + Qualcomm QNN setup, Gemma 3 (4B int4) loading, EmbeddingGemma 128-dim MRL pipeline, PII guardrails.
* **Track C (Connectivity & Backend Lead):** Firebase Auth, Nearby Connections BLE/Wi-Fi Direct engine, Go/Node.js Match Relay microservice.

---

## Hour-by-Hour Execution Timeline

```
[ Hours 0-6: Setup ] -> [ Hours 6-12: Core Dev ] -> [ Hours 12-18: Mid Integrations ]
                                                                   |
[ Hours 24-30: Polish & Demo ] <- [ Hours 18-24: End-to-End Test ] <-+
```

### Hours 00 – 06: Architecture Setup & Skeleton Build
* **Track A:** Initialize Android project skeleton (Kotlin, Compose, Hilt DI, Material 3, Navigation Graph).
* **Track B:** Download Gemma 3 (4B int4) and EmbeddingGemma from Qualcomm AI Hub; verify LiteRT initialization on iQOO 15 hardware.
* **Track C:** Spin up Firebase project, configure Firebase Auth, and write lightweight Node.js WebSocket Match Relay server.

### Hours 06 – 12: Core Feature Construction
* **Track A:** Build Phase 0 Registration screens, Hinge-style profile builder (photos, prompts, goal selector).
* **Track B:** Wire Gemma 3 into local chat ViewModel; achieve real-time streaming output on Snapdragon NPU (~28 tokens/sec).
* **Track C:** Implement Nearby Connections API manager for BLE discovery and Wi-Fi Direct channel creation.

### Hours 12 – 18: Feature Integration & Data Pipeline
* **Track A:** Build Phase 1 AI discovery chat UI with rolling daily summarization progress indicators.
* **Track B:** Build EmbeddingGemma vector extraction pipeline; implement MRL compression down to 128 dimensions; construct cosine similarity math engine.
* **Track C:** Connect client to Cloud Match Relay; test vector submission and candidate fetching over HTTPS.

### Hours 18 – 24: Guardrails & Tap-to-Connect Integration
* **Track A:** Implement Phase 4 anonymous chat UI with `FLAG_SECURE` window protection.
* **Track B:** Build Tier 1 Regex + Tier 2 Gemma Prompt Guard PII filtering pipeline; wire blocking warnings and cooldown timers.
* **Track C:** Complete iQOO-to-iQOO Tap-to-Connect offline vector exchange & direct chat bootstrap over Wi-Fi Direct.

### Hours 24 – 30: System Integration, Polish & Pitch Preparation
* **Track A, B & C:** Execute full 5-phase end-to-end user walk-through testing on physical iQOO 15 test devices.
* **Bug Squashing:** Fix NPU memory allocation leaks, adjust Compose UI animations, refine "101 Cosmic Match" visual badge presentation.
* **Demo Recording & Pitch Prep:** Capture screen video recordings and prepare live hardware demonstration setup.

---

## Real Code vs. Mocked Feature Matrix

| Feature Component | MVP Hackathon Status | Technical Implementation Details |
| :--- | :--- | :--- |
| **Phase 0 Registration & Profile** | **REAL CODE** | Fully functional Jetpack Compose screens, Firebase Auth, Room DB. |
| **Phase 1 On-Device Gemma 3 Chat** | **REAL CODE** | Executes locally on Snapdragon NPU via LiteRT + Qualcomm QNN. |
| **Phase 2 EmbeddingGemma Vector** | **REAL CODE** | Real 300M embedding reduced to 128-dim MRL vector. |
| **Phase 3 Vector Match Math** | **REAL CODE** | Cosine similarity calculation executed locally on client. |
| **Phase 3 Cosmic 101 Badge** | **REAL CODE** | Verified top 0.5% threshold + $$\ge 2$$ rare tag math evaluation. |
| **Phase 4 PII Guardrails** | **REAL CODE** | Real Tier 1 Regex + Tier 2 Gemma Guard + `FLAG_SECURE`. |
| **Tap-to-Connect BLE/Wi-Fi** | **REAL CODE** | Functional Nearby Connections P2P transport between iQOO hardware. |
| **Phase 4 Day 7 Unlock Delay** | **MOCKED / SIMULATED** | Accelerated to 30 seconds for live demo presentation. |
| **Global User Match Pool** | **MOCKED / SIMULATED** | Seeded with 50 realistic candidate vectors on local relay server. |
