# Delta Hedge Bot / Delta 对冲机器人

> 在 Predict 上卖出期权的同时在 Hyperliquid 永续上 Delta 对冲，赚取纯 Vol Edge

## 解决什么问题

在 Predict 上卖出区间期权可以赚取权利金，但裸卖风险极大。Delta Hedge Bot 在卖出 Predict 期权的同时，在 Hyperliquid 永续合约上进行精确的 Delta 对冲，使整个仓位的 PnL 纯粹由波动率差异驱动，而非方向性风险。

## 核心功能

- 在 Predict 上自动卖出区间期权（predict::mint）
- 实时计算每个仓位的 Delta，在 Hyperliquid 开等量反向永续仓位
- 定期再平衡：当 Delta 偏移超过阈值时调整永续仓位
- 支持 Hyperliquid Delta 对冲的完整生命周期管理

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::mint | 卖出区间期权收取权利金 |
| predict::redeem_permissionless | 期权到期后赎回 |
| PredictManager | 管理所有 Predict 仓位 |
| OracleSVI | 计算初始 Delta 和再平衡触发条件 |

## 实现方案

### 智能合约 (Move)
- `delta_hedge_vault` 模块：资金管理、仓位记录
- `hedge_state` 模块：记录链上链下对冲状态
- 关键数据结构：`HedgePosition`（Predict 仓位 Delta、Hyperliquid 对冲量、净 Delta）

### Bot / Keeper / 服务端
- Delta 计算器：根据 Binary 期权定价模型计算实时 Delta
- Hyperliquid 执行器：通过 API 管理永续仓位
- 再平衡引擎：当净 Delta 超过阈值时提交调整
- 风控模块：最大单仓暴露、止损线、异常检测

### 前端 / 后端
- 技术栈：React + Recharts + Python
- 跨平台仓位仪表盘：Predict 仓位 + Hyperliquid 仓位
- 净 Delta 实时曲线和再平衡历史
- Vol Edge PnL 追踪

## 技术架构

Bot 在 Predict 上卖出区间（predict::mint）→ 立即计算 Delta → 通过 Hyperliquid API 开反向永续仓位 → 持续监控 BTC 价格和 Delta 变化 → 偏移超阈值时再平衡 → Predict 到期时 redeem → 平仓 Hyperliquid → 结算 Vol Edge 收益。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 基础仓位记录 |
| Bot/算法 | ⭐⭐⭐⭐⭐ | Delta 计算和跨平台对冲是核心 |
| 前端 | ⭐⭐⭐ | 跨平台可视化 |
| 集成复杂度 | ⭐⭐⭐⭐ | 链上 Predict + 链下 Hyperliquid 联动 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
