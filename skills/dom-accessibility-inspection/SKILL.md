---
name: "dom-accessibility-inspection"
description: "Inspects DOM hierarchies and extracts accessible element trees with stable locator references."
---

# DOM & Accessibility Inspection

## Overview
This skill enables agents to inspect complex web page structures through standard accessibility APIs rather than raw, noisy HTML markup.

## Key Capabilities
- **ARIA Hierarchy Parsing**: Extracts semantic roles (button, link, textbox, combobox) and their current states (checked, expanded, disabled).
- **Element Handle Binding**: Computes stable `@e1`, `@e2` shorthand locators for precise prompt injection.
- **Visual Occlusion Detection**: Determines whether target elements are visible within the current viewport or obscured by popups.

## Operational Workflow
1. **Tree Querying**: Query the browser accessibility tree via CDP `Accessibility.getFullAXTree`.
2. **Interactive Filtering**: Filter nodes to retain interactive controls and informative text landmarks.
3. **Handle Generation**: Index active nodes with ephemeral sequential reference tokens.
4. **Context Injection**: Provide the sanitized, compact element table to the agent reasoning loop.
