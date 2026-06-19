# Range Ladder Vault / 阶梯区间金库

> 自动在 ATM 行权价周围部署一列 Predict 区间，到期自动滚动，发行代币化份额

## 解决什么问题

散户和机构都无法便捷地在链上执行"卖出波动率"策略。现有预测市场只支持单次手动下单，缺乏自动化滚动和仓位管理。Range Ladder Vault 让用户存入 dUSDC 后自动在平值附近部署区间阶梯，持续收取权利金，到期无缝续期。

## 核心功能

- 自动计算当前 ATM 行权价，按可配置宽度部署 5-10 个区间（predict::mint）
- 到期时自动结算并滚动到下一期，无需手动干预
- 发行代币化份额（VaultToken），可在 Sui DeFi 中转让和组合
- 支持三种行权宽度策略：固定基点、SVI 1σ 动态、基于已实现波动率自适应

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::mint | 在每个到期日 mint 区间期权 |
| predict::redeem_permissionless | 到期后自动赎回已结算区间 |
| PredictManager | 管理金库的所有仓位 |
| OracleSVI | 读取 SVI 曲线动态调整行权宽度 |
| deepbook 现货 | 处理实值区间滚动时的 dUSDC 兑换 |

## 实现方案

### 智能合约 (Move)
- `range_ladder_vault` 模块：管理用户存款、份额铸造/销毁
- `ladder_strategy` 模块：计算 ATM 行权价、生成区间列表、执行 mint
- `vault_token` 模块：符合 Sui Token 标准的份额代币
- 关键数据结构：`Vault`（总资产、份额总量、当前区间列表）、`LadderConfig`（宽度、档位数、策略类型）

### Bot / Keeper / 服务端
- Keeper 每 30 秒检查是否有区间到期，触发滚动 PTB
- 滚动逻辑：redeem 旧区间 → 读取最新预言机价格 → 计算新 ATM → mint 新区间
- 监控深度实值/虚值区间的处理逻辑（平仓或保留）

### 前端 / 后端
- 技术栈：Next.js + Tailwind + @mysten/dapp-kit
- 存款/取款页面，实时展示当前区间分布图
- APY 计算面板，历史收益曲线
- 后端用索引器追踪所有仓位状态

## 技术架构

用户通过前端存入 dUSDC → Vault 合约记录份额 → Keeper 定期检查到期事件 → 到期时 Keeper 提交 PTB（redeem 旧区间 + mint 新区间）→ 预言机价格更新触发结算 → 收益累积到 Vault 中。前端通过索引器 API 实时展示 Vault 净值和区间分布。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐⭐ | 需要处理多区间管理、代币化、滚动逻辑 |
| Bot/算法 | ⭐⭐⭐ | ATM 计算和滚动调度需精确 |
| 前端 | ⭐⭐⭐ | 区间分布可视化和 APY 计算 |
| 集成复杂度 | ⭐⭐⭐⭐ | 多区间 PTB 编排是核心挑战 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
