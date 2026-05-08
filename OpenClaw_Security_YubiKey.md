---
title: "OpenClaw Security: 远程系统级 YubiKey 提权"
date: 2026-03-08
---

# 远程系统级硬件鉴权 (Remote System-Level Auth)

为了实现在聊天软件中授权，同时保持“系统级”安全性，我们使用 `pam_yubico` 模块。

# 核心原理：PAM Yubico OTP

不同于 `pam_u2f` 需要物理接触，`pam_yubico` 接受一段由 YubiKey 生成的 44 位 ModHex 字符串（OTP），并由操作系统（PAM 层）直接向 Yubico 云端验证其合法性。

### 1. 系统级防御 (NixOS)
即使 AI 进程被注入攻击，如果它拿不到你手中 YubiKey 生成的实时 OTP，它绝对无法执行 `sudo`。

### 2. NixOS 配置 (`/etc/nixos/configuration.nix`)
```nix
{ config, pkgs, ... }: {
  security.pam.services.sudo.rules.auth.yubico = {
    order = 1; # 最先验证
    control = "required";
    modulePath = "${pkgs.pam_yubico}/lib/security/pam_yubico.so";
    # id 是你的 Yubico API ID, key 是 API Key
    args = [ "id=12345" "key=base64_secret" "debug" ];
  };

  environment.systemPackages = [ pkgs.yubico-pam ];
}
```

### 3. 聊天授权流程 (The Chat-to-System Bridge)

1. **AI 发起**: OpenClaw 执行 `sudo nixos-rebuild switch`。
2. **系统拦截**: PAM 模块 `pam_yubico` 暂停进程，在终端等待输入 `YubiKey OTP`。
3. **AI 转达**: OpenClaw 检测到提示，发消息给你：*"系统正在请求硬件授权。请在对话框输入你的 YubiKey OTP（触摸 Key 即可生成）。"*
4. **用户操作**: 你在手机聊天窗口点击 YubiKey（或连接手机），生成一串字符并发送。
5. **AI 注入**: OpenClaw 将这串字符输入到 `sudo` 的 stdin 中。
6. **PAM 验证**: 操作系统直接连接 Yubico 服务器验证。成功则命令执行。

## 为什么这比 LLM Guard 更安全？
- **内核级拦截**: 验证逻辑在操作系统内核/PAM 路径中，而不是在 AI 的 Python 代码里。
- **不可伪造**: 只有你手里的物理硬件能产生合法的 OTP 序列。
- **防止重放**: 每个 OTP 只能使用一次，AI 无法存储旧的 OTP 来偷偷提权。

## 参考文献
- [[Task_Chat_OTP_Sudo]] - 详细操作步骤
