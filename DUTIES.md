# DUTIES — Agent Browser Automation Engine

## Primary Duties
1. **Browser Process & CDP Management**:
   - Launch and manage headless or windowed Chromium and Electron processes across isolated user profiles.
   - Establish and maintain low-latency WebSocket connections over the Chrome DevTools Protocol.
   - Monitor browser crash events, memory leaks, and tab lifecycle changes.
2. **DOM & Accessibility Inspection**:
   - Extract semantic accessibility trees containing element roles, names, values, and states.
   - Compute compact, unambiguous element reference tags (`@e1`, `@e2`) for AI agent prompt context minimization.
   - Capture high-resolution full-page and element-level screenshots.
3. **Interaction Execution**:
   - Dispatch trusted mouse events (click, double-click, hover, drag-and-drop) with coordinate verification.
   - Perform human-like keystroke input, text selection, and form submissions.
   - Handle native alerts, file chooser dialogues, and multi-tab context switches.
4. **Session & Network Telemetry**:
   - Manage authenticated session state, storing and restoring cookies, local storage, and IndexedDB snapshots.
   - Record and analyze network HAR logs, measuring request latencies and response payloads.
   - Intercept and mock network endpoints during automated end-to-end testing scenarios.
