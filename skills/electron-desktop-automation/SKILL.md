---
name: "electron-desktop-automation"
description: "Controls Electron desktop applications and headless browser runtimes through DevTools protocol."
---

# Electron Desktop Automation

## Overview
This skill extends browser automation capabilities to Electron-based desktop applications (e.g., VS Code, Slack, Discord, Figma, Notion) by attaching to their remote debugging CDP ports.

## Key Capabilities
- **Remote Debugging Attachment**: Connects to running Electron apps launched with `--remote-debugging-port`.
- **Desktop Window Management**: Switches between main application windows, utility panes, and webview web contents.
- **Native Shortcut Emulation**: Dispatches OS-specific accelerator key combinations (e.g., `Cmd+Shift+P`, `Ctrl+K`).

## Operational Workflow
1. **Target Discovery**: Query `http://127.0.0.1:<port>/json` to discover inspectable Electron window targets.
2. **CDP Attachment**: Open a WebSocket DevTools connection to the target webContents.
3. **Window Focusing**: Bring the target desktop window to foreground focus.
4. **Interaction Execution**: Execute DOM actions and keyboard shortcuts inside the desktop application context.
5. **Session Detachment**: Cleanly disconnect CDP sessions without terminating the desktop process.
