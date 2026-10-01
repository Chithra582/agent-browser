# EXPLAINABILITY — Agent Browser Automation Engine

> **Admissibility & Transparency Report for OpenGAP / Agent Passport**  
> *Agent Name:* Agent Browser Automation Engine (`agent-browser`)  
> *Specification:* OpenGAP v0.1.0  
> *Domain:* Developer Tools / Browser Automation & Web AI  

---

## 1. Overview & Operational Purpose
The **Agent Browser Automation Engine** is an autonomous browser automation runtime and Chrome DevTools Protocol (CDP) bridge designed specifically for AI agents. It replaces heavyweight and brittle browser wrappers with a fast, native automation CLI that inspects web pages via semantic accessibility trees, assigns compact element handles, and executes verified synthetic user interactions.

Its operational purpose is to enable AI agents to perform complex, multi-step browser workflows—including form navigation, web scraping, authenticated SaaS workflows, Electron desktop app control, and regression testing—reliably and securely.

---

## 2. How the Agent Decides (Decision-Making Logic)
Agent Browser Automation Engine operates across a deterministic, multi-stage decision pipeline:

```
[Stage 1: Intent Parsing] ──> [Stage 2: Target Resolution] ──> [Stage 3: Safety Verification]
                                                                       │
                                                                       ▼
[Stage 6: Artifact Return] <── [Stage 5: State Confirmation] <── [Stage 4: CDP Execution]
```

### 2.1 Intent Parsing & Protocol Mapping
- **Decision:** Parse incoming agent commands (e.g., click, type, navigate, snapshot) and map them to standard CDP domains (Page, DOM, Input, Runtime, Network).
- **Rules:** Reject unrecognized commands, validate required parameters against the JSON schema, and reject malformed URLs or untrusted schemas.

### 2.2 Target Resolution & Handle Binding
- **Decision:** Resolve target web elements against the cached semantic accessibility tree using unique `@eN` references or ARIA locators.
- **Rules:** If an element reference has been invalidated by DOM mutations, trigger an immediate tree re-indexing and update the handle mapping before proceeding.

### 2.3 Safety Verification & Policy Check
- **Decision:** Evaluate target element role and action type against safety policies and sensitive action classifications.
- **Rules:** Halt execution and request human authorization if the action involves financial checkouts, account deletion buttons, or unmasked credential exports.

### 2.4 CDP Execution & State Confirmation
- **Decision:** Dispatch low-level CDP protocol messages and verify successful execution via DOM mutation events and network quiescence.
- **Rules:** Wait for the page lifecycle event (`networkidle` or `domcontentloaded`) before emitting operation status and returning downstream artifacts.

---

## 3. Data & Privacy
| Data Category | Retention Policy | Third-Party Sharing | Storage Mechanism |
|---|---|---|---|
| Rendered DOM & Accessibility Trees | Ephemeral (Command Lifecycle) | None | In-Memory CLI Buffers |
| Session Cookies & Storage Vaults | Encrypted (User Configured) | None | Local Encrypted SQLite / Keyring |
| Captured Screenshots & HAR Logs | Ephemeral / Debug Run | None | Local Output Directory (`/artifacts/`) |
| Network Request & Response Headers | Debug Session Duration | None | In-Memory Rolling Buffer |

Agent Browser Automation Engine complies with operational security and privacy standards:
- **No Cloud Data Exfiltration:** All browser automation routines, DOM trees, user credentials, and network logs remain strictly local to the runtime host environment without unauthorized external transmission.
- **Epistemic Isolation:** Each browser automation session executes in an isolated temporary user profile directory, preventing cross-session credential leakage or tracking persistence.
- **Sanitized Model Payloads:** Accessibility-tree snapshots provided to upstream AI models are automatically stripped of hidden password fields, security tokens, and unnecessary DOM clutter.
- **Data Minimization:** The engine captures only the minimal accessibility subtree and network events required to fulfill the specific automation directive.

---

## 4. Known Limitations & Failure Modes
Reviewers, auditors, and users should note the following operational constraints:
1. Dynamic Single-Page App Hydration Delays
   - *Limitation:* Web pages with heavy client-side JavaScript hydration or asynchronous component loading may render element references before event listeners attach.
   - *Mitigation:* The engine enforces automatic retry backoffs and checks element interactability flags before dispatching input events.
2. Anti-Bot Captcha Interventions
   - *Limitation:* Cloudflare Turnstile, reCAPTCHA, and custom bot detection mechanisms may block automated browser sessions.
   - *Mitigation:* The engine detects challenge markers, pauses execution gracefully, and signals the host agent or human supervisor for interactive resolution.
3. Canvas and WebGL Rendering Blindspots
   - *Limitation:* Applications rendered entirely inside HTML5 Canvas, WebGL, or WebGPU do not expose native DOM accessibility nodes.
   - *Mitigation:* The engine provides coordinate-based fallback targeting and screenshot capture to allow multimodal vision-based action planning.
4. Memory Consumption in Long-Running Sessions
   - *Limitation:* Navigating hundreds of continuous web pages in a single browser context can cause Chromium memory footprints to balloon.
   - *Mitigation:* The engine actively monitors process RSS memory, closes inactive background tabs, and recycles browser instances periodically.

---

## 5. Verification, Safety & Human Oversight
Agent Browser Automation Engine integrates multi-layer safety rails to ensure full human accountability and system integrity:
- **Real-Time Human Approval Gate:** Destructive form submissions, financial transactions, and credential disclosures require explicit affirmative human confirmation.
- **Emergency Session Interrupt:** Any running browser session can be immediately paused, frozen, or closed via standard terminal signal interrupts (`SIGINT`, `SIGTERM`).
- **Step Quota Guardrails:** Strict configurable operation limits (e.g., max 50 actions per session) prevent infinite navigation loops or runaway automation scripts.
- **Structured Audit Logging:** Every executed CDP command, navigated URL, DOM modification, and network request is recorded in a local, timestamped audit log.
