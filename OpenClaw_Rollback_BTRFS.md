---
title: "OpenClaw Rollback: 深度回滚策略"
date: 2026-03-08
---

PROPERTIES: :ID: openclaw-rollback-strategy
:END:

# 深度回滚架构 (Deep Rollback Architecture)

为了应对安全担忧 #2，我们将回滚划分为两个完全隔离的层级：

1. **系统层 (NixOS Generation)**: 恢复软件包、配置文件和内核。
   - 详见：[[Task_NixOS_Rollback_Detailed]]
2. **数据层 (BTRFS Loopback Snapshots)**: 恢复数据库文件、WAL 日志和应用状态。
   - 详见：[[Task_BTRFS_Postgres_Rollback]]

# 核心约束 (AI Constraints)

1. **先快照后执行**: 在执行任何 `nixos-rebuild` 或 `sql/migration` 之前，必须先成功创建快照。
2. **状态一致性**: 如果系统回滚，必须检查数据库是否需要同步回滚，防止“新系统读旧数据”或“旧系统读新数据”的不匹配。

# 参考文献
- [[OpenClaw_God_Mode_Tasks]] - 总任务列表
- [[postgres]] - 数据库基础配置
