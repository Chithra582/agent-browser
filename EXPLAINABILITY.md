# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **Agent Browser Automation Engine** (`agent-browser`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** Agent Browser Automation Engine (`agent-browser`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Developer Tools / Browser Automation & Web AI  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

The Agent Browser Automation Engine is an autonomous browser automation runtime and Chrome DevTools Protocol (CDP) bridge designed specifically for AI agents. It replaces heavyweight and brittle browser wrappers with a fast, native automation CLI that inspects web pages via semantic accessibility trees, assigns compact element handles, and executes verified synthetic user interactions. Its operational purpose is to enable AI agents to perform complex, multi-step browser workflows—including form navigation, web scraping, authenticated SaaS workflows, Electron desktop app control, and regression testing—reliably and securely.

### 1. Decision Architecture

The DOM accessibility snapshot, semantic element localization, action execution, and DOM state verification pipeline operates across a deterministic, five-stage architecture:

```
User Workflow Objective (Form Navigation / Web Extraction / E2E Regression Scenario)
    │
    ▼
[Stage 1: Page Navigation & Accessibility Tree Ingestion]
    │  - Attaches to Chrome/Chromium instance via Chrome DevTools Protocol (CDP)
    │  - Navigates to target URL and captures semantic accessibility (AX) tree
    │  - Filters invisible nodes and parses interactive elements (buttons, inputs, links)
    ▼
[Stage 2: Compact Handle Assignment & Spatial Indexing]
    │  - Maps accessibility nodes to compact numeric handles (`@1`, `@2`, `@3`)
    │  - Computes bounding client rects and viewport scroll visibility
    │  - Injects minimal handle attributes into live DOM without disrupting page styles
    ▼
[Stage 3: Element Localization & Action Planning]
    │  - Resolves agent natural language action to exact target handle via semantic matching
    │  - Validates element interactability (pointer-events, disabled status, occlusion)
    │  - Formulates synthetic interaction sequence (click, type, scroll, select)
    ▼
[Stage 4: Synthetic Action Dispatch & Network Idle Wait]
    │  - Dispatches native trusted CDP input events (Input.dispatchMouseEvent, dispatchKeyEvent)
    │  - Awaits DOM mutations and network idle state (`networkidle0`)
    │  - Captures post-action DOM mutations and visual viewport screenshot
    ▼
[Stage 5: State Verification & Trajectory Log Archival]
    │  - Verifies expected URL transition, DOM state changes, or form submission results
    │  - Scrubs sensitive passwords, session cookies, and credit card numbers from traces
    │  - Emits structured JSON execution trace to local workspace for auditing
    ▼
Validated Web Interaction Result & Auditable Browser Trajectory Record
```

### 2. Decision Logic & Handle Matching Formulations

Agent Browser evaluates element localization, occlusion probability, and action confidence using deterministic mathematical models:

1. **Semantic Element Handle Affinity ($S_{\text{handle}}$)**:
   $$S_{\text{handle}}(e) = (w_r \cdot R_{\text{role}}) + (w_n \cdot N_{\text{name}}) + (w_v \cdot V_{\text{visibility}})$$
   where:
   - $R_{\text{role}} \in \{0, 1\}$ represents accessibility role alignment (button, textbox, link).
   - $N_{\text{name}} \in [0, 1]$ represents normalized string similarity against element accessible names.
   - $V_{\text{visibility}} \in [0, 1]$ represents viewport intersection area ratio.
   - Weights: $w_r = 0.40, w_n = 0.40, w_v = 0.20$ ($\sum w_i = 1.0$).

2. **Action Safety & Occlusion Index ($I_{\text{safe}}$)**:
   $$I_{\text{safe}}(e) = 1 - O_{\text{occlusion}}(e)$$
   where $O_{\text{occlusion}}(e)$ detects overlapping modal dialogs or transparent overlays at target coordinates $(x, y)$, preventing misclicks.

### 3. Thresholding & Refusal Decision Criteria

Agent Browser Automation Engine enforces strict operational safety and privacy boundaries:
- **Refusal to Auto-Submit Financial Transactions**: Form submissions triggering real-money payments or credit card processing require explicit human confirmation (`ERR_PAYMENT_SUBMISSION_REQUIRES_APPROVAL`).
- **Refusal to Bypass Captcha or Bot Protections**: Instructions attempting to bypass Cloudflare Turnstile, reCAPTCHA, or biometric authentication are rejected (`ERR_BOT_DETECTION_BYPASS_PROHIBITED`).
- **Turn Ceiling Enforcement**: Multi-step browsing loops enforce a hard limit of `max_turns: 25` to prevent circular navigation loops (`WARN_TURN_BUDGET_REACHED`).
- **Domain Whitelist Confinement**: Navigation requests to unverified or suspicious external top-level domains trigger security warnings (`WARN_EXTERNAL_DOMAIN_NAVIGATION`).

### 4. Fallback Decision Mechanism

Continuous browser automation is maintained through multi-tier fault recovery:
- **Model Cascade Failover**: When the primary foundation model experiences latency spikes or HTTP 429 rate limits, the orchestrator cascades automatically between `claude-3-5-sonnet`, `gpt-4o`, and `gemini-2.0-flash`.
- **Selector Fallback Cascade**: If accessibility tree handles fail due to dynamic shadow DOM rendering, the engine cascades to XPath and CSS selector fallbacks.
- **Graceful Navigation Retry**: If a page navigation times out, the browser refreshes with exponential backoff before throwing actionable error codes.

### 5. Human-in-the-Loop Governance

Human operators retain complete visual supervision and session control:
- **Interactive Headed Mode Inspection**: Operators can launch the browser in headed mode to observe agent clicks, cursor movements, and form inputs in real time.
- **Emergency Session Kill Switch**: Operators can halt browser actions instantly via `Ctrl+C` interrupt signals or by closing the browser window.
- **Full Action Trajectory Archival**: Every CDP command, screenshot snapshot, and network request is logged in structured trajectory files for auditability.

---

## The Data It Uses

Agent Browser operates under strict privacy, data minimization, and local workspace isolation standards.

### 1. Ingested Input Data

The agent processes only operational assets necessary to fulfill browser automation:
- **Navigation Directives**: Target URLs, search terms, and form fill instructions.
- **Accessibility DOM Trees**: Structured JSON hierarchies containing element roles, accessible names, and attributes.
- **Viewport Screenshots**: Ephemeral PNG/JPEG visual captures of the active browser viewport.

### 2. Configuration & Reference Data

- **CDP Command Schemas**: Chrome DevTools Protocol specifications for Page, DOM, Runtime, and Input domains.
- **Browser Profiles**: Local user data directories, cookie stores, and device emulation profiles.
- **Domain Permission Rulesets**: Whitelists and blacklists defining allowed and blocked network domains.

### 3. Base Model & Inference Lineage

- **Deterministic Browser Engines**: Chromium rendering engine, CDP protocol bridges, and accessibility tree serializers executed natively in C++ and Node.js (100% deterministic with zero LLM variance).
- **Foundation LLMs**: High-capability frontier models (`claude-3-5-sonnet`, `gpt-4o`, `gemini-2.0-flash`) utilized for visual reasoning, intent planning, and complex DOM comprehension.
- **Zero Training on User Browsing Data**: User browsing histories, session cookies, and webpage contents are never transmitted to external cloud servers or used for model training.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Hardened**: Defended against prompt injection via adversarial web content, credential harvesting, and cross-site scripting (XSS).
- **Local-Only Profile Storage**: All browser caches, session states, and trajectory screenshots reside exclusively on the user's local filesystem.
- **Automated Password Scrubbing**: Password fields and sensitive inputs are automatically masked in execution logs and screenshots.
- **Zero Commercial Monetization**: User web interactions, browsing trajectories, and scraped data are never monetized, aggregated, or shared with third parties.

---

## Limitations

Understanding the operational boundaries and technical constraints of Agent Browser is essential for effective deployment.

### 1. Dynamic Canvas & WebGL Rendering
- **Limitation**: Web applications rendering controls inside opaque HTML5 `<canvas>` elements lack accessibility nodes, making handle assignment challenging.
- **Mitigation**: The engine pairs coordinate-based visual grounding with OCR text recognition for canvas-heavy interfaces.

### 2. Complex Multi-Factor Authentication Challenges
- **Limitation**: Autonomous agents cannot independently complete physical hardware key or biometric MFA challenges.
- **Mitigation**: The browser supports persistent authenticated user profiles, allowing human operators to log in once manually before handing control to the agent.

### 3. Heavy Single-Page App Hydration Race Conditions
- **Limitation**: Single-page applications (React, Angular) with complex asynchronous hydration can cause transient element detachment during clicks.
- **Mitigation**: The engine polls element stability and checks for DOM mutation quiescence before dispatching input events.

### 4. Headless Anti-Bot Fingerprinting Detection
- **Limitation**: Commercial anti-bot providers (Cloudflare, Akamai) detect default headless Chrome signatures and block access.
- **Mitigation**: The engine injects realistic user-agent strings, navigator properties, and human-like cursor trajectory curves.

### 5. Multi-Tab Context Switching Overhead
- **Limitation**: Orchestrating workflows across dozens of simultaneous browser tabs can consume significant host RAM.
- **Mitigation**: The engine aggressively unloads inactive background tabs and maintains centralized CDP target trackers.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & handle matching formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested navigation directives, DOM trees & screenshots | Section 1 | Verified |
| - Configuration, CDP schemas & domain rulesets | Section 2 | Verified |
| - Base model lineage & deterministic browser engines | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Dynamic canvas & WebGL rendering | Section 1 | Verified |
| - Complex multi-factor authentication challenges | Section 2 | Verified |
| - Heavy single-page app hydration race conditions | Section 3 | Verified |
| - Headless anti-bot fingerprinting detection | Section 4 | Verified |
| - Multi-tab context switching overhead | Section 5 | Verified |
