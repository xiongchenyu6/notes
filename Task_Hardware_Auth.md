---
title: "Task: Hardware-Signed Sudo"
date: 2026-03-08
---

# Instruction for the AI: Gating Sudo

This document defines how I (the AI) must handle credential and privilege escalation requests to meet security concern #1.

## Operational Workflow

1. **Detection**: Identify when a user-requested action requires `sudo` or root permissions.
2. **Challenge Generation**:
   - Send a specific message to the chat channel (Telegram/Signal):
   - *"I am about to execute a privileged command: `<command>`. Please provide your YubiKey signature."*
3. **Verification**:
   - Call the `otp-challenger` skill to verify the incoming OTP against the Yubico API.
   - Do NOT cache or store the OTP.
4. **Execution**:
   - Only execute the `sudo` command upon successful verification.
   - Use the `NOPASSWD` sudoer rule *only* when this flow has been completed.

## References
- [[OpenClaw_Security_YubiKey]] - Architecture
- [[OpenClaw_God_Mode_Tasks]] - Main Directives
