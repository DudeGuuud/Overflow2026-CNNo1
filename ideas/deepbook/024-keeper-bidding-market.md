# Keeper Bidding Market / Keeper 竞价市场

> 多个 Keeper 竞争执行赎回任务，最低小费者获胜，去中心化执行层

## 解决什么问题

Predict 的 redeem_permissionless 允许第三方代为赎回，但目前缺乏竞争机制来降低用户成本。Keeper Bidding Market 创建一个去中心化的任务竞价市场，多个 Keeper 竞争执行赎回任务，以最低小费报价者获胜。这种竞争机制确保用户支付最低的赎回费用，同时激励 Keeper 通过优化 Gas 和效率来竞争。

## 核心功能

- 链上竞价订单簿：Keeper 提交小费报价，最低价者获得执行权
- 自动任务匹配：新赎回任务发布后自动触发竞价
- 执行担保：Keeper 需质押保证金，未按时执行则罚没
- 绩效排行榜：Keeper 成功率、平均执行时间、总收益

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::redeem_permissionless | 竞价获胜 Keeper 执行赎回 |
| PTB | 竞价 + 赎回打包为原子交易 |
| Sui Coin | 保证金质押和小费结算 |
| Shared Object | 链上竞价状态管理 |

## 实现方案

### 智能合约 (Move)
- `bidding_market` 模块：任务发布、竞价提交、执行验证
- `keeper_staking` 模块：Keeper 保证金质押和罚没
- 关键数据结构：`Task`（赎回目标、截止时间）、`Bid`（Keeper、小费金额）、`KeeperProfile`（质押、成功率）

### Bot / Keeper / 服务端
- 任务扫描器：检测新的可赎回仓位
- 竞价策略器：根据 Gas 成本和竞争状况计算最优报价
- 执行器：竞价获胜后调用 redeem_permissionless
- 罚没监控器：检测超时未执行任务并触发罚没

### 前端 / 后端
- 技术栈：React + Sui Move + Node.js
- 竞价面板：当前任务、竞价历史、小费趋势
- Keeper 注册和管理界面
- 用户视角：赎回成本预估和 Keeper 选择

## 技术架构

任务扫描器发现可赎回仓位 → 链上发布赎回任务 → 多 Keeper 提交竞价 → 最低小费者获胜 → 获胜 Keeper 执行 redeem_permissionless → 链上验证执行并结算小费 → 超时则罚没保证金。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐⭐ | 链上竞价逻辑和保证金管理 |
| Bot/算法 | ⭐⭐⭐ | 竞价策略和执行优化 |
| 前端 | ⭐⭐⭐ | 竞价面板和 Keeper 管理 |
| 集成复杂度 | ⭐⭐⭐⭐ | 链上链下协调和竞争逻辑 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
