---
title: "Task: BTRFS & Postgres 细化回滚流程"
date: 2026-03-08
---

# AI 指令: 数据库快照与恢复

由于 Postgres 在 `ext4` 系统上难以做到数据一致性回滚，AI (我) 必须管理好 BTRFS loopback 卷。

## 1. 快照管理流程 (Snapshot Management)

在任何变更之前：
```bash
# 停止数据库服务 (强制性，防止内存数据未同步)
sudo systemctl stop postgresql

# 创建快照
sudo btrfs subvolume snapshot /var/lib/postgresql/18/data /var/lib/postgresql/18/snap_pre_$(date +%Y%m%d%H%M)

# 启动服务
sudo systemctl start postgresql
```

## 2. 细化回滚步骤 (Postgres-Safe Rollback)

如果数据库变更导致错误、数据损坏或迁移失败：
1. **停止服务**: `sudo systemctl stop postgresql`
2. **清理锁文件**: 删除任何残留的 `postmaster.pid`
3. **隔离损坏数据**: `mv /var/lib/postgresql/18/data /var/lib/postgresql/18/data_broken_$(date +%s)`
4. **从快照恢复**: 
   ```bash
   btrfs subvolume snapshot /var/lib/postgresql/18/snap_pre_<ID> /var/lib/postgresql/18/data
   ```
5. **权限校准**: `sudo chown -R postgres:postgres /var/lib/postgresql/18/data`
6. **重启并验证**: `sudo systemctl start postgresql`

## 3. 数据库一致性检查 (Integrity Checks)
回滚完成后，AI 必须：
- 运行 `psql -c "SELECT version();"` 确保连接正常。
- 检查 `journalctl -u postgresql` 的错误日志。
- 执行一个特定的验证 SQL (由用户在 [[Task_Database_Management]] 中定义)。

## 4. 故障排查
如果回滚失败：
- 立即上报：*"回滚失败：快照 `<ID>` 不存在或 BTRFS loopback 已损坏。"*
- 绝不要在回滚失败时尝试格式化或删除原数据。

## 参考文献
- [[postgres]] - 数据库配置
- [[OpenClaw_Rollback_BTRFS]] - 总体策略
