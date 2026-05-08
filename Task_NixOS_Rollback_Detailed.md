---
title: "Task: NixOS 细化回滚流程"
date: 2026-03-08
---

# AI 指令: NixOS 系统恢复

当 AI (我) 的系统更改导致错误时，我必须按照以下精确步骤恢复系统。

## 1. 识别损坏级别 (Damage Assessment)
- **非破坏性故障** (e.g. 进程启动失败): 在当前环境下回滚。
- **破坏性故障** (e.g. 连通性丧失、系统挂起): 要求用户在引导菜单选择回滚。

## 2. 操作流程 (Operational Steps)

### A. 检查当前生成 (Generations)
```bash
sudo nix-env --list-generations --profile /nix/var/nix/profiles/system
```

### B. 执行回滚 (Standard Rollback)
```bash
sudo nixos-rebuild switch --rollback
```

### C. 锁定已知好版本 (Pinning)
为了防止垃圾回收 (`nix-collect-garbage`) 意外删除上一个已知稳定的版本，AI 必须将该版本临时锁定：
```bash
# 生成一个名为 'stable-before-ai-god-mode' 的新 profile
sudo nix-env -p /nix/var/nix/profiles/system-stable --set /nix/var/nix/profiles/system-X-link
```

## 3. 故障排查
如果回滚失败：
- 记录 `/var/log/messages` 或 `journalctl -u nixos-rebuild`。
- 请求用户介入并指引：*"系统配置由于 `<reason>` 损坏，请在重启后从引导菜单选择前一个版本。"*

## 参考文献
- [[nix]] - 基础 tips
- [[OpenClaw_Rollback_BTRFS]] - 总体策略
