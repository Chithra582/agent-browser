---
name: "agent-browser"
description: "Fast browser automation CLI for AI agents via CDP with accessibility-tree snapshots and element refs."
---

# Agent Browser CLI Automation

## Overview
This skill provides the primary automation interface for autonomous AI agents navigating websites and web applications using the fast native `agent-browser` CLI.

## Key Capabilities
- **Fast CDP Integration**: Communicates directly with Chrome/Chromium instances via native DevTools Protocol sockets.
- **Accessibility Snapshotting**: Generates compact, token-efficient semantic accessibility trees with stable `@eN` references.
- **Deterministic Action Dispatch**: Executes verified clicks, keystrokes, form fills, and navigation events.

## Operational Workflow
1. **Session Initialization**: Launch or attach to a Chromium instance in headless or visual mode.
2. **Page Navigation**: Navigate to the target URL and wait for DOM and network quiescence.
3. **Accessibility Snapshot**: Capture the interactive accessibility tree and resolve relevant element references.
4. **Action Execution**: Execute required interactions using verified element handles.
5. **State Verification**: Confirm URL changes, DOM mutations, and visual feedback before continuing.
