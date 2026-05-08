---
title: "Task: Headless NixOS 自动化切换与恢复"
date: 2026-03-08
---

# AI 指令: 远程系统高安全切换流程

Headless 环境下，AI (我) 的首要任务是 **“防止失联”**。

## 1. 系统配置变更的“安全网” (The Safety Net)

执行任何 `sudo nixos-rebuild switch` 之前，必须先使用 `test` 命令。

### 脚本流程
```bash
# 1. 创建快照和回滚任务
echo "sudo nixos-rebuild switch --rollback" | at now + 5 minutes

# 2. 应用测试配置 (不写入引导，重启即恢复)
sudo nixos-rebuild test

# 3. 验证网络和服务
if ping -c 3 8.8.8.8 && systemctl is-active sshd; then
    # 如果一切正常，清理回滚任务并正式应用
    atrm $(atq | cut -f1)
    sudo nixos-rebuild switch
else
    # 如果测试失败，手动回滚并上报
    sudo nixos-rebuild switch --rollback
fi
```

## 2. Headless BTRFS 数据库回滚细化

针对 Postgres 的回滚，Headless 必须特别注意 **“在线恢复”**。

### 细化步骤
1. **停止进程**: `sudo systemctl stop postgresql`
2. **挂载点检查**: 确保 `/var/lib/pg_btrfs.img` 的 loopback 依然在线。
   - `losetup -a | grep pg_btrfs.img`
3. **快照回滚 (在线模式)**:
   - 绝不要卸载 loopback 卷。
   - 在已挂载的 BTRFS 内部重命名 subvolume：
   ```bash
   mv /var/lib/postgresql/18/data /var/lib/postgresql/18/data_failed_$(date +%s)
   btrfs subvolume snapshot /var/lib/postgresql/18/snap_stable /var/lib/postgresql/18/data
   ```
4. **验证重启**: 重新拉起服务。

## 3. SSH 安全冗余 (Connectivity Persistence)

即使正在修改网络设置，也要确保 SSH 不受影响。
- **验证脚本**: 在应用变更后，AI 必须尝试 `ssh -o ConnectTimeout=5 localhost` 验证服务是否存活。

## 参考文献
- [[OpenClaw_Context_Engineering]] - 上下文总纲
- [[Task_BTRFS_Postgres_Rollback]] - 数据库基础回滚
