# Stablecoin Enhanced Vault / 稳定币增强金库

> USDC 存入 PLP 赚取基础收益，收益部分自动投入低风险区间策略增强回报

## 解决什么问题

稳定币持有者希望获得高于传统 DeFi 借贷的收益，但直接参与预测市场风险不可控。稳定币增强金库将大部分资金安全地供应给 PLP 赚取稳定收益，仅将收益部分投入低风险 Predict 区间策略，实现"本金安全 + 收益增强"的风险收益结构。

## 核心功能

- 95% 资金通过 predict::supply 供应 PLP，赚取稳定基础收益
- 5% 收益自动转入低风险区间策略（宽区间、短到期）
- 分层风险管理：PLP 层安全，策略层有限暴露
- 实时展示分层收益和风险指标

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::supply | 将大部分资金供应给 PLP |
| predict::mint | 用收益部分买入低风险区间 |
| predict::redeem_permissionless | 区间到期后赎回 |
| PredictManager | 管理 PLP 和策略仓位 |
| OracleSVI | 选择波动率低谷时部署策略 |

## 实现方案

### 智能合约 (Move)
- `enhanced_vault` 模块：分层资金管理、收益自动分配
- `yield_router` 模块：将 PLP 收益路由到策略层
- 关键数据结构：`VaultState`（PLP 层余额、策略层余额、累计收益）、`AllocationConfig`（PLP 比例、策略比例）

### Bot / Keeper / 服务端
- 收益收集器：定期检查 PLP 收益，将新增收益转入策略层
- 策略执行器：在策略层部署低风险宽区间
- 风险监控：确保策略层暴露不超过设定上限

### 前端 / 后端
- 技术栈：Next.js + Chart.js
- 分层资产展示：PLP 层 vs 策略层
- 收益瀑布图：基础收益 + 增强收益 = 总收益
- 风险评估面板

## 技术架构

用户存入 USDC → 95% predict::supply 给 PLP → 5% 用于策略 → Keeper 定期从 PLP 收益中提取 → 策略层 predict::mint 买入宽区间 → 到期 redeem → 收益回流到 Vault。前端实时展示两层资产状态。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 分层资金管理和收益路由 |
| Bot/算法 | ⭐⭐ | 简单的分配和低风险策略 |
| 前端 | ⭐⭐ | 分层展示 |
| 集成复杂度 | ⭐⭐⭐ | PLP + 区间组合 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
