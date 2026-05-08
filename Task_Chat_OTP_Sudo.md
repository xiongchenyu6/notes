---
title: "Task: 通过聊天中继 YubiKey OTP 进行 sudo"
date: 2026-03-08
---

# AI 指令: 聊天授权中继器

本任务文档定义了 AI (我) 如何在聊天软件 (Telegram/Signal) 中处理用户的 YubiKey OTP。

## 操作流程 (Operational Workflow)

1. **识别执行需要**: 检测到任务需要 `sudo` 权限。
2. **触发授权请求**:
   - 向用户发送一条具体的消息：
   - *"正在执行：`<command>`。操作系统需要硬件授权。请在聊天中发送一段由 YubiKey 生成的 OTP (触摸物理按键即可)。"*
3. **输入流拦截 (STDIN Hijack)**:
   - 使用 `pexpect` 或类似的 PTY 机制打开 `sudo`。
   - 等待终端出现 `YubiKey OTP: ` 的提示。
4. **获取用户回复**:
   - 监听聊天软件中用户的下一条消息。
   - 提取该 44 位的 ModHex 字符串。
5. **注入系统**:
   - 将用户发送的 OTP 字符串写入到 `sudo` 进程的 stdin 中。
   - 如果系统反馈错误 (Authentication Failure)，立即中止任务。

## 注意事项
- 绝不要将 OTP 记录在日志或 `memory.md` 中。
- 绝不要在用户未发送 OTP 之前尝试盲目输入。
- 只有 44 位以 `cccccc` 开头的字符串才被视为 YubiKey OTP。

## 参考文献
- [[OpenClaw_Security_YubiKey]] - 系统级架构
- [[OpenClaw_God_Mode_Tasks]] - 总任务列表
