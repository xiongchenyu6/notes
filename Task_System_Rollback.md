---
title: "Task: BTRFS & NixOS Rollback"
date: 2026-03-08
---

# Instruction for the AI: Filesystem & System State

This document defines how I (the AI) must perform state-reverting actions for security concern #2.

## Operational Workflow

1. **Pre-Flight Snapshot**:
   - For **Database Tasks**:
     - `sudo btrfs subvolume snapshot /var/lib/postgresql/18/data /var/lib/postgresql/18/snap_pre_<task_name>`
   - For **System/OS Tasks**:
     - `sudo nixos-rebuild switch` (This creates a generation automatically)
2. **Execute State Change**:
   - Run the user-requested database migration or system configuration.
3. **Validation**:
   - Verify success (exit code 0).
   - If a failure occurs, I must automatically trigger the rollback:
     - Database: `mv data data_failed && btrfs subvolume snapshot snap_pre_<task_name> data`
     - System: `sudo nixos-rebuild switch --rollback`

## References
- [[OpenClaw_Rollback_BTRFS]] - Architecture
- [[OpenClaw_God_Mode_Tasks]] - Main Directives
