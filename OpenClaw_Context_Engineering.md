---
title: "OpenClaw 上下文工程: Headless 安全准则"
date: 2026-03-08
---

# 核心指令: 远程服务器安全操作 (Headless Mandates)

你正在一台 **Headless NixOS 服务器** 上操作。你无法访问物理显示器或引导菜单。任何网络或 SSH 配置错误都将导致永久失联。

## 1. 变更执行逻辑: "试运行" 模式 (Trial Pattern)

在执行 `nixos-rebuild` 时，禁止直接使用 `switch`。必须遵循以下阶梯：

1. **Dry Run**: `sudo nixos-rebuild build` (验证语法和依赖)。
2. **Test Mode**: `sudo nixos-rebuild test` (应用变更但不写入引导，重启即恢复)。
3. **连通性验证**: 在 `test` 之后，必须验证 SSH 和网关连通性。
4. **Finalize**: 只有验证通过后，才执行 `sudo nixos-rebuild switch`。

## 2. 自动化回滚预案 (Auto-Rollback Logic)

如果在执行变更后 60 秒内，你无法通过网关与用户通信，你必须假设系统已断网。
- **预埋指令**: 在 `test` 变更前，在后台运行一个“自杀开关”脚本：
  ```bash
  # 如果 5 分钟后系统没被确认，自动重启以恢复前一个生成
  echo "sleep 300 && reboot" | at now
  ```

## 3. 数据库快照隔离 (BTRFS Isolation)

由于是 Headless，数据库的回滚必须是“在线”的。
- 永远保持 `data_previous` 符号链接指向最新的稳定快照。
- 发生错误时，通过修改符号链接而非移动大量文件来瞬间切换版本。

## 4. 异常报告模板
若授权通过但执行超时，立即向用户发送：
- *"警告：变更已应用但系统响应变慢。正在监控网络状态。预置回滚脚本已激活。"*

## 参考文献
- [[Task_Headless_NixOS_Safety]] - 技术实现细节
- [[Task_Chat_OTP_Sudo]] - 远程授权流程
