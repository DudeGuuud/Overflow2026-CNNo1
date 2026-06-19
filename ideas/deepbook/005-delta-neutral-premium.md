# Delta Neutral Premium Collection / Delta 中立权利金收割

> 持续卖出区间期权，收取权利金，通过 DeepBook 现货 Delta 对冲

## 解决什么问题

链上缺乏成熟的"卖出波动率"策略执行工具。交易者需要手动计算 Delta、在多个平台操作对冲，无法实现真正的 Delta 中立。本项目自动化整个流程：在 Predict 上卖出区间，通过 DeepBook 现货自动对冲 Delta，让用户稳定收取权利金。

## 核心功能

- 自动在 ATM 附近卖出 Predict 区间，收取权利金
- 实时计算组合 Delta，当偏离阈值时通过 DeepBook 现货再平衡
- 支持多到期日同时运行，优化资金效率
- 可配置的风险参数：最大 Delta 偏移、再平衡频率、止损线

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::mint | 卖出区间期权（收取权利金） |
| predict::redeem_permissionless | 结算后赎回赔付 |
| deepbook 现货 | Delta 对冲的现货买卖 |
| PredictManager | 管理所有卖出的区间仓位 |
| OracleSVI | 判断当前波动率水平是否适合卖出 |

## 实现方案

### 智能合约 (Move)
- `delta_neutral_vault` 模块：用户存款、份额管理
- `hedge_engine` 模块：计算组合 Delta、触发再平衡
- 关键数据结构：`HedgeState`（总 Delta、现货对冲量、区间仓位列表）、`RebalanceParams`（Delta 阈值、再平衡步长）

### Bot / Keeper / 服务端
- Delta 计算器：每分钟更新组合 Delta
- 再平衡执行器：当 Delta 偏移超过阈值时提交 PTB（现货买入/卖出）
- 到期处理器：结算区间 + 评估是否需要调整现货对冲

### 前端 / 后端
- 技术栈：React + D3.js（Delta 曲线可视化）
- 实时 Delta 仪表盘，再平衡历史记录
- 权利金收益曲线和风险归因分析

## 技术架构

用户存入 dUSDC → Vault 卖出 Predict 区间（mint）→ Bot 持续计算组合 Delta → Delta 偏移时 Bot 通过 DeepBook 现货对冲 → 区间到期时 redeem → 净收益（权利金 - 对冲成本 - 赔付）累积到 Vault。整个循环由 Keeper 自动驱动。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 区间管理 + 份额代币 |
| Bot/算法 | ⭐⭐⭐⭐⭐ | Delta 计算和再平衡策略是核心 |
| 前端 | ⭐⭐⭐ | Delta 和权利金可视化 |
| 集成复杂度 | ⭐⭐⭐⭐ | Predict + 现货对冲联动 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
