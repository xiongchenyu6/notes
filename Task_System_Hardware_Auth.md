---
title: "Task: System-Level YubiKey Sudo"
date: 2026-03-08
---

# Instruction for the AI: PAM-Gated Execution

This document defines how I (the AI) must handle privilege escalation, which is enforced at the **NixOS PAM layer** for absolute security.

## Operational Workflow

1. **Identification**: Detect the need for `sudo`.
2. **Notification to User**:
   - Send a message to the chat (Telegram/Signal):
   - *"Executing: `<command>`. Your YubiKey is flashing. Please TOUCH it to authorize."*
3. **Trigger Sudo**:
   - Execute the `sudo` command. The command will HANG in the terminal while waiting for the physical touch.
4. **Physical Interlock**:
   - Only when the human (User) touches the physical YubiKey will the OS (PAM) allow the command to proceed.
   - If no touch occurs within the timeout, the command fails.

## NixOS Configuration (The System-Level Guard)

I must ensure the following is configured in `/etc/nixos/configuration.nix`:

```nix
{ pkgs, ... }: {
  # Enforce YubiKey for all sudo attempts
  security.pam.services.sudo.u2fAuth = true;

  # Do NOT use NOPASSWD for sensitive commands
  # This ensures the OS always triggers the YubiKey prompt
}
```

## User Registration (Initial Setup)
The user must register their YubiKey once:
```bash
mkdir -p ~/.config/Yubico
pamu2fcfg > ~/.config/Yubico/u2f_keys
```

## References
- [[OpenClaw_Security_YubiKey]] - System-Level Architecture
- [[OpenClaw_God_Mode_Tasks]] - Core Directives
