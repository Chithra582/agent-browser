---
name: "auth-vault-session-management"
description: "Manages authenticated browser sessions, persistent cookie state, and credentials vault."
---

# Auth Vault & Session Management

## Overview
This skill manages authenticated user sessions, cookies, localStorage, and authentication tokens, enabling agents to operate across authenticated web sessions without repeated manual login flows.

## Key Capabilities
- **Session Persistence**: Saves and restores cookies, localStorage, and sessionStorage snapshots across runs.
- **Encrypted Token Vault**: Protects sensitive authentication state using local AES encryption or OS keychain.
- **Domain Scoping**: Restricts session data to specific domain boundaries to prevent credential leakage.

## Operational Workflow
1. **Authentication Capture**: Capture current session cookies and storage tokens following a successful login.
2. **Vault Storage**: Serialize and encrypt session artifacts to local vault storage under a designated profile name.
3. **Session Restoration**: Inject stored cookies and tokens into a freshly initialized browser profile prior to navigation.
4. **Expiry Verification**: Check token and cookie validity timestamps, triggering refresh workflows when expired.
