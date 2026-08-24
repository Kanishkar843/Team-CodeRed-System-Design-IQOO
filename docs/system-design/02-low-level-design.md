# 02 — Low-Level Design (LLD)

> [!NOTE]
> **TL;DR**  
> **Who cares:** Mobile software developers, QA automation leads, and technical reviewers.  
> **What it does:** Details low-level execution logic and sequence flows across SoulSync's 5 critical operational phases and PII guardrails.  
> **Why this approach:** Guarantees deterministic execution across on-device NPU pipelines, local storage transactions, and network relays.  
> **What it costs:** 100-300ms local NPU execution latency during vector generation; zero network overhead for LLM chat.

---

## Acronym Glossary

* **LLD (Low-Level Design):** Detailed component diagrams and workflow specifications describing exact method execution order.
* **UUID (Universally Unique Identifier):** A 128-bit label used to uniquely identify records across computer systems.
* **MRL (Matryoshka Representation Learning):** Nested vector embedding compression technique preserving semantic similarity at lower dimensions.
* **Regex (Regular Expression):** A sequence of characters specifying a search pattern for text matching and validation.
* **NPU (Neural Processing Unit):** On-device silicon processor optimized for neural network inference.

---

## Critical Flow 1: Registration & Profile Setup (Phase 0)

```mermaid
sequenceDiagram
    autonumber
    actor User as Mobile User
    participant UI as Jetpack Compose UI
    participant VM as Auth ViewModel
    participant Auth as Firebase Auth SDK
    participant DB as Encrypted Room DB

    User->>UI: 1. Enter Phone Number
    UI->>VM: 2. Submit Verification Request
    VM->>Auth: 3. Send OTP Request
    Auth-->>VM: 4. OTP Sent Signal
    User->>UI: 5. Input OTP Code
    UI->>VM: 6. Verify OTP Code
    VM->>Auth: 7. Validate Credentials
    Auth-->>VM: 8. Auth Success (Returns ID Token)
    User->>UI: 9. Fill Prompts & Goal (Soulmate/Casual)
    UI->>VM: 10. Save Profile Package
    VM->>DB: 11. Insert Encrypted Profile Record
    DB-->>UI: 12. Confirm Phase 0 Complete
```

### Plain-English Explanation
> **Read in 60 seconds:** When you sign up, you verify your phone number using Firebase. You then answer setup prompts and pick your relationship goal (soulmate or casual). Everything you type is stored inside an encrypted vault on your phone. Nothing is sent to a cloud chat server.

---

## Critical Flow 2: On-Device 2-Day AI Chat & Rolling Summarization (Phase 1)

```mermaid
sequenceDiagram
    autonumber
    actor User as Mobile User
    participant UI as Chat Screen View
    participant NPU as LiteRT + QNN Delegate
    participant Gemma as Gemma 3 (4B QAT int4)
    participant Summarizer as Daily Summarizer Task
    participant DB as Encrypted Room DB

    rect rgb(249, 228, 234)
    note right of User: Active On-Device Conversation
    User->>UI: 1. Send Dialogue Text
    UI->>NPU: 2. Dispatch Prompt Context Buffer
    NPU->>Gemma: 3. Execute NPU Stream Inference (~28 tok/s)
    Gemma-->>UI: 4. Stream Response Tokens to Screen
    UI->>DB: 5. Save Encrypted Conversation Turn
    end

    rect rgb(231, 155, 175)
    note right of User: Daily Session Summarization
    UI->>Summarizer: 6. Trigger End-of-Day Session Summary
    Summarizer->>NPU: 7. Extract Psychological Traits
    NPU-->>DB: 8. Update Profile Summary Digest
    end
```

### Plain-English Explanation
> **Read in 60 seconds:** For two days, you chat naturally with Gemma 3, an AI living inside your phone's processor. The AI asks questions about your values and daily life. At night, the AI summarizes the key personality points into a compact daily log and saves it securely in your local vault.

---

## Critical Flow 3: Personality Vector Extraction & MRL Compression (Phase 2)

```mermaid
sequenceDiagram
    autonumber
    participant Manager as Phase 2 Manager
    participant DB as Encrypted Room DB
    participant EmbedEngine as EmbeddingGemma (300M)
    participant MRL as MRL Compression Module
    participant Cache as Local Vector Storage

    Manager->>DB: 1. Fetch 2-Day Session Summary Digest
    DB-->>EmbedEngine: 2. Provide Text Summaries & Prompts
    EmbedEngine->>EmbedEngine: 3. Generate 768-dim Embedding Vector
    EmbedEngine->>MRL: 4. Truncate & Normalize to 128-dim MRL
    MRL->>Cache: 5. Store Encrypted 128-dim Vector & Niche Tags
    Cache-->>Manager: 6. Confirm Phase 2 Complete
```

### Plain-English Explanation
> **Read in 60 seconds:** Once the 2-day chat finishes, SoulSync takes your profile answers and daily summaries and feeds them into EmbeddingGemma (an on-device AI model). It creates a 768-number map of your personality, then instantly shrinks it to a compact 128-number code using Matryoshka compression. This 128-number code hides raw details while preserving your core personality traits.

---

## Critical Flow 4: Compatibility Matching & "101 Cosmic Match" Rarity Evaluation (Phase 3)

```mermaid
sequenceDiagram
    autonumber
    participant ClientA as Client App A
    participant Relay as Cloud Match Relay
    participant ClientB as Client App B
    participant Math as Local Vector Math Engine

    ClientA->>Relay: 1. Submit Anonymized 128-dim Vector & Niche Tags
    ClientB->>Relay: 2. Submit Anonymized 128-dim Vector & Niche Tags
    Relay-->>ClientA: 3. Deliver Candidate Vector B
    ClientA->>Math: 4. Compute Cosine Similarity: score = round((cos + 1)/2 * 100)
    Math->>Math: 5. Evaluate Rarity: Is Score in Top 0.5% AND Shared Tags >= 2?
    
    alt Rarity Met (Top 0.5% & >= 2 Niche Tags)
        Math-->>ClientA: 6a. Award "101 Cosmic Match" Badge!
    else Standard Match
        Math-->>ClientA: 6b. Display Standard Compatibility Score %
    end
```

### Plain-English Explanation
> **Read in 60 seconds:** Your phone sends its anonymized 128-number code to a tiny relay server, which swaps it with potential matching partners. Your phone receives candidate codes and calculates compatibility using vector math locally. If two people score in the top 0.5% AND share at least 2 rare niche hobbies, both phones award a special "101 Cosmic Match" badge!

---

## Critical Flow 5: Phase 4 Anonymous 7-Day Bonding Window & On-Device PII Guardrail Enforcement

```mermaid
sequenceDiagram
    autonumber
    actor User as Sender User
    participant UI as Chat View (FLAG_SECURE)
    participant Regex as Tier 1 Regex Interceptor
    participant GemmaGuard as Tier 2 Gemma Prompt Guard
    participant Relay as Encrypted WebSocket Relay
    actor Partner as Recipient Match Partner

    User->>UI: 1. Type "Call me at 9876543210"
    UI->>Regex: 2. Scan Text for Phone/Email Patterns
    Regex-->>UI: 3. MATCH DETECTED! BLOCK Immediately & Warn User
    
    User->>UI: 4. Type "Find my Insta handle john_doe"
    UI->>Regex: 5. Scan Text (Passes Tier 1)
    UI->>GemmaGuard: 6. Semantic Evaluation (Prompt Guard)
    GemmaGuard-->>UI: 7. DETECTED SOCIAL HANDLE! BLOCK & Mute User
    
    User->>UI: 8. Type "I love reading sci-fi books!"
    UI->>Regex: 9. Scan Text (PASS)
    UI->>GemmaGuard: 10. Semantic Check (PASS)
    UI->>Relay: 11. Transmit Encrypted Payload
    Relay-->>Partner: 12. Deliver Anonymous Message
```

### Plain-English Explanation
> **Read in 60 seconds:** During the 7-day anonymous window, you chat with your match using anonymized IDs. Before any message leaves your phone, an on-device safety guard scans it. If you try to send phone numbers, emails, or social media handles, the message is instantly blocked locally, and a warning is shown. Screenshotting is physically blocked by Android system flags (`FLAG_SECURE`).

---

## Component Rationale (WHAT, WHY, WHAT BREAKS WITHOUT IT)

### 1. Daily Summarizer
* **What it is:** An NPU background routine that condenses long chat history into bulleted psychological summaries.
* **Why it exists:** Keeps the LLM prompt context within memory budget limits without losing long-term user context.
* **What would break without it:** Gemma 3 would run out of memory or forget conversation details from Day 1.

### 2. MRL Compressor
* **What it is:** A vector transformation algorithm reducing 768-dim embeddings down to 128-dim vectors.
* **Why it exists:** Minimizes network payload bandwidth and hides raw feature dimensions for maximum privacy.
* **What would break without it:** Vector exchange would take 6x more bandwidth and expose excessive user metadata.

### 3. Regex & Gemma Guard Combination
* **What it is:** A 2-tiered filtering system combining instant pattern matching with local LLM semantic analysis.
* **Why it exists:** Regex catches obvious numbers instantly, while Gemma Guard catches sneaky workarounds (e.g., "nine eight seven six").
* **What would break without it:** Users could easily bypass safety rules during the anonymous 7-day bonding window.
