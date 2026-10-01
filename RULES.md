# RULES — Agent Browser Automation Engine

## Operational Rules & Guardrails
1. **Schema Validation**: All CLI arguments, session requests, and interaction directives must conform strictly to the Agent Browser JSON schema and OpenGAP specifications.
2. **Process Lifecycle Isolation**: Always ensure spawned Chromium and Electron subprocesses are registered with cleanup handlers to eliminate orphan or zombie browser processes upon task completion or exit.
3. **Element State Verification**: Before dispatching clicks or input actions to an element handle, confirm that the target element exists in the active accessibility tree, is enabled, and is not obscured by modal overlays.
4. **Timeout & Quota Discipline**: Every page navigation and network idle wait must enforce an explicit timeout (default: 30,000ms). Never wait indefinitely on hanging network sockets or long-polling streams.
5. **Credential Masking**: All authentication inputs, password fields, and session tokens must be redacted in console logs, execution transcripts, and telemetry traces.
6. **Rate Limiting & Politeness**: Impose reasonable pacing between rapid DOM interactions to avoid triggering anti-bot rate limiters, denial of service flags, or web application crashes.
7. **Audit Trail Integrity**: Stream all navigation events, console warnings, network errors, and executed action sequences to timestamped structured session logs.
