# PLP + Hedge Vault / PLP 对冲金库

> 供应 dUSDC 赚取 PLP 收益，同时买入虚值二元期权对冲尾部风险

## 解决什么问题

纯 PLP 供应虽然收益可观，但在 BTC 剧烈波动时 LP 面临重大回撤风险。现有方案缺少内置的下行保护机制，导致保守型资金不敢参与。PLP + Hedge Vault 通过自动将部分收益用于购买虚值看跌二元期权，构建"收益 + 保险"产品，降低极端场景下的最大回撤。

## 核心功能

- 一键存入 dUSDC，自动通过 predict::supply 成为 PLP
- 按可配置比例（如 10-20% 收益）定期购买虚值二元期权作为尾部对冲
- 动态调整对冲比例：SVI 偏高时减少对冲、SVI 偏低时增加对冲
- 实时展示净 APY（PLP 收益 - 保险成本）和历史回撤对比

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::supply | 将 dUSDC 供应给 PLP 赚取收益 |
| predict::mint | 定期购买虚值二元期权（尾部对冲） |
| predict::redeem_permissionless | 对冲期权到期后自动赎回赔付 |
| PredictManager | 统一管理 PLP 仓位和对冲期权仓位 |
| OracleSVI | 读取波动率曲面判断对冲成本是否合理 |

## 实现方案

### 智能合约 (Move)
- `plp_hedge_vault` 模块：存款、份额管理、策略参数配置
- `hedge_manager` 模块：计算对冲需求、执行 mint 操作
- 关键数据结构：`VaultState`（PLP 供应量、对冲仓位列表、累计收益）、`HedgeConfig`（对冲比例、行权价偏移量）

### Bot / Keeper / 服务端
- 定时任务：每周/每周期从 PLP 收益中提取一部分用于对冲
- 对冲选择逻辑：根据当前 BTC 价格和 SVI 曲线，选择最优虚值行权价和到期日
- 到期处理：赎回到期对冲，如触发赔付则补充 Vault 净值

### 前端 / 后端
- 技术栈：React + ECharts + @mysten/dapp-kit
- 仪表盘：展示 PLP 收益曲线、对冲成本、净 APY
- 情景模拟器：±5σ 场景下有/无对冲的 PnL 对比
- 后端计算实时 APY 和历史指标

## 技术架构

用户存入 dUSDC → Vault 合约调用 predict::supply → Keeper 定期检查收益累积 → 触发对冲 PTB（从 Vault 提取收益 → predict::mint 购买 OTM 期权）→ 对冲到期时 Keeper 调用 redeem_permissionless → 赔付（如有）回流到 Vault。前端实时展示策略状态和风险指标。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | PLP supply + mint 组合逻辑 |
| Bot/算法 | ⭐⭐⭐⭐ | 动态对冲比例计算和 SVI 判断 |
| 前端 | ⭐⭐⭐ | APY 计算和风险可视化 |
| 集成复杂度 | ⭐⭐⭐ | 两个 Predict 功能的组合 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
