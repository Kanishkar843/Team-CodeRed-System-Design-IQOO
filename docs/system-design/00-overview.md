# 00 — Product Overview & System Vision

> [!NOTE]
> **TL;DR**  
> **Who cares:** Hackathon judges, product managers, privacy auditors, and non-engineering evaluators.  
> **What it does:** Outlines SoulSync’s product vision, core problem statement, phase-based user lifecycle, and architectural boundaries.  
> **Why this approach:** Solves online dating fatigue and privacy violations by executing 100% of LLM personality analysis locally on the smartphone NPU.  
> **What it costs:** Zero server-side AI compute cost; requires an Android device with Snapdragon NPU acceleration.

---

## Acronym Glossary

To ensure clarity for all evaluators, the following technical terms are defined upon first usage:

* **NPU (Neural Processing Unit):** Dedicated hardware silicon inside modern mobile processors optimized for matrix math and deep neural network acceleration.
* **LLM (Large Language Model):** Generative artificial intelligence models capable of understanding and synthesizing natural language dialogues.
* **PII (Personally Identifiable Information):** Any user data that can disclose identity, such as phone numbers, email addresses, handles, or live photos.
* **MRL (Matryoshka Representation Learning):** A machine learning technique that embeds high-dimensional vectors (e.g., 768 dimensions) into nested lower dimensions (e.g., 128 dimensions) without severe accuracy loss.
* **QNN (Qualcomm AI Engine Direct):** Qualcomm's software development kit enabling low-level execution of neural networks directly on Snapdragon NPUs.
* **FCM (Firebase Cloud Messaging):** Google's cross-platform messaging solution for delivering push notification alerts to mobile devices.
* **DPDP Act (Digital Personal Data Protection Act 2023):** India’s primary statutory data privacy regulation governing personal data processing.

---

## Problem Statement & Product Vision

Modern online dating applications suffer from three systemic flaws:

1. **Superficial Swipe Fatigue:** User evaluation relies almost entirely on visual presentation, leading to shallow interactions and rapid user churn.
2. **Privacy & Data Exploitation:** Centralized servers store sensitive intimate conversations, making user profiles vulnerable to data breaches, unauthorized data scraping, and third-party monetization.
3. **Behavioral Inauthenticity:** Gamified interfaces incentivize users to project idealized personas rather than revealing authentic psychological traits.

### The SoulSync Solution
**SoulSync** is an AI-powered social connection platform that transforms relationship discovery. Instead of swiping through photo grids, users participate in a **2-day conversational discovery window** guided by an on-device AI assistant powered by **Gemma 3 (4B QAT int4)**. The local NPU synthesizes conversational nuances into a privacy-preserving **128-dimensional Matryoshka personality vector**. 

Matches are calculated using local mathematical vector comparison, ensuring that **no cloud LLM ever sees or stores user chat content**.

```
+-------------------------------------------------------------------------------+
|                             SOULSYNC PRIVACY PROMISE                          |
|                                                                               |
|   +--------------------+     +---------------------+     +----------------+   |
|   | User Chat Dialogue | --> | On-Device NPU (Gemma)| --> | 128-dim Vector |   |
|   +--------------------+     +---------------------+     +----------------+   |
|            |                            |                        |            |
|            X (NEVER LEAVES DEVICE)      X (LOCAL INFERENCE ONLY)   v            |
|                                                          [ Minimal Cloud ]    |
+-------------------------------------------------------------------------------+
```

---

## Target Audience & Hardware Specifications

SoulSync is engineered mobile-first for modern flagship Android hardware, with an eventual path to iOS via Swift.

### Primary Hardware Benchmark: iQOO 15
* **System on Chip (SoC):** Qualcomm Snapdragon 8 Elite Gen 5.
* **NPU Architecture:** Hexagon NPU featuring a ~37% performance boost in neural matrix math compared to prior generations `[assumption — verify against Qualcomm benchmark announcements]`.
* **System Memory:** Up to 16 GB LPDDR5X RAM (allocating ~2.5 GB dedicated VRAM/RAM for Gemma 3 int4 and ~300 MB for EmbeddingGemma).
* **Thermal Target:** Continuous NPU execution constrained under 1.2W power draw to maintain cool thermal performance during active dialogue sessions.

### Graceful Fallback Target: Snapdragon 8 Gen 2+ Devices
* Devices equipped with Snapdragon 8 Gen 2 or Gen 3 dynamically reduce Gemma 3 execution precision and throttle prompt processing speed (tokens/sec) to preserve thermal stability while retaining full functional compatibility.

---

## The 5-Phase User Lifecycle

SoulSync structures user onboarding and interaction into five distinct operational phases:

```
[Phase 0: Registration] --> [Phase 1: 2-Day AI Talk] --> [Phase 2: Vector Extract]
                                                                  |
[Phase 4: 7-Day Anon Window] <-- [Phase 3: Compatibility Match] <--+
```

### Phase 0 — Registration & Base Profile Setup
* User registers via Firebase Authentication.
* User builds a profile specifying photos, prompt answers, core preferences, age verification (18+), and relationship goals (**Soulmate** vs. **Casual**).

### Phase 1 — 2-Day On-Device AI Discovery Conversation
* User engages in guided natural language dialogue with an on-device instance of Gemma 3 (4B QAT int4).
* The AI asks psychological, lifestyle, and values-based questions.
* At the end of each daily session, an NPU summarization algorithm condenses the conversation into rolling profile memory.

### Phase 2 — On-Device Personality Vector Extraction
* Upon completing the 2-day discovery period, EmbeddingGemma 300M processes the profile data and dialogue summaries.
* Outputs a 768-dimensional embedding, which is MRL-compressed into a privacy-preserving **128-dimensional vector** representing psychological traits and niche interests.

### Phase 3 — Compatibility Matching & Cosmic Rarity
* The client exchanges 128-dim vectors via a minimal cloud relay.
* Compatibility score is derived using normalized cosine similarity:
  $$\text{score} = \text{round}\left(\frac{\text{cosine\_similarity}(A, B) + 1}{2} \times 100\right)$$
* **"101 Cosmic Match" Badge:** Awarded exclusively when the similarity score ranks in the top **0.5%** of the user base AND the pair shares **$$\ge 2$$ rare niche tags** (a gamified rarity mechanic; the underlying mathematical score never exceeds 100%).

### Phase 4 — 7-Day Anonymous Bonding Window
* Matched users enter a 7-day anonymous chat channel where only anonymized User IDs are displayed.
* **On-Device PII Guardrail:** Continuously monitors outbound messages using regex, Gemma prompt guards, and `FLAG_SECURE` window protection to block sharing of phone numbers, emails, social handles, or photos.
* **Day 7 Unlocking:** After 7 days of verified interaction, both users can mutually consent to unlock true profiles, direct contact information, voice calls, and video calls.

---

## Bonus Feature: iQOO-to-iQOO Tap-to-Connect

For instant in-person discovery (e.g., at events or social gatherings), SoulSync incorporates **Tap-to-Connect**:
* Utilizes Android Nearby Connections API for offline Bluetooth Low Energy (BLE) discovery.
* Automatically establishes a high-bandwidth Wi-Fi Direct peer-to-peer (P2P) channel between two iQOO devices.
* Performs offline local vector exchange, compatibility score calculation, and direct chat bootstrapping without requiring active cellular internet coverage.

---

## Scope & Non-Goals

### In-Scope for Architectural Design
1. Complete Android client system design using Kotlin, Jetpack Compose, and Room SQLite.
2. Full technical integration spec for Qualcomm AI Engine Direct (QNN) and LiteRT.
3. Complete security model for on-device PII guardrails and privacy-preserving vector exchange.
4. Detailed data schemas, REST/WebSocket API endpoints, and 30-hour hackathon execution plan.

### Explicit Non-Goals
1. **Cloud LLM Hosting:** Server-side LLM inference is explicitly excluded from the architecture.
2. **Centralized Chat Backup:** Server-side retention of user chat messages is strictly prohibited.
3. **Web Client Platform:** Browser-based desktop clients are out of scope for the mobile-first strategy.
