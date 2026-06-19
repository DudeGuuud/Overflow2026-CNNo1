# BTC Collateral Predict Vault / BTC 抵押预测金库

> 接受 BTC 抵押，通过 DeepBook 现货换成 dUSDC，运行方向性策略，归还 BTC 收益

## 解决什么问题

BTC 持有者希望在不出售 BTC 的前提下参与 Predict 市场获取收益，但目前没有一站式解决方案。BTC Collateral Vault 让用户存入 BTC（xBTC/sBT），自动兑换为 dUSDC 后运行 Predict 策略，最终以 BTC 计价返还收益，解决跨资产管理和 FX 风险定价的痛点。

## 核心功能

- 接受 BTC（xBTC、sBTC 等封装资产）作为抵押品存入
- 通过 DeepBook 现货自动将 BTC 兑换为 dUSDC
- 支持两种策略模式：Delta 中性权利金收割、方向性动量
- 到期自动结算，按策略收益换回 BTC 并返还用户

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::mint | 使用兑换后的 dUSDC 执行方向性策略 |
| predict::redeem_permissionless | 策略到期后自动赎回 |
| deepbook 现货 | BTC/dUSDC 双向兑换 |
| PredictManager | 管理 Vault 的所有 Predict 仓位 |
| OracleSVI | 用于 Delta 中性策略的波动率参考 |

## 实现方案

### 智能合约 (Move)
- `btc_vault` 模块：接受 BTC 存入、铸造份额、管理兑换流程
- `strategy_router` 模块：根据用户选择切换策略模式
- 关键数据结构：`BtcVault`（BTC 锁定量、dUSDC 余额、策略类型）、`Position`（当前 Predict 仓位详情）

### Bot / Keeper / 服务端
- 入金时触发：BTC → dUSDC 兑换 + 策略部署（一个 PTB 完成）
- 定时监控：BTC/dUSDC 汇率变化，动态调整抵押率
- 到期处理：赎回 Predict 仓位 → 换回 BTC → 更新用户余额

### 前端 / 后端
- 技术栈：Next.js + @mysten/dapp-kit
- BTC 存款页面，实时 BTC 计价净值
- 策略选择器（权利金收割 vs 动量），历史 BTC 计价收益
- 风险提示：展示 FX 敞口和最大损失情景

## 技术架构

用户存入 BTC → Vault 合约锁定 BTC → PTB 通过 DeepBook 现货兑换为 dUSDC → 根据策略 predict::mint 买入区间 → 到期时 redeem → PTB 将 dUSDC 换回 BTC → 用户提取 BTC + 收益。全程通过 PTB 保证原子性。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐⭐ | 跨资产管理 + PTB 编排 |
| Bot/算法 | ⭐⭐⭐ | FX 风险管理和策略切换 |
| 前端 | ⭐⭐ | 标准 DeFi 前端 |
| 集成复杂度 | ⭐⭐⭐⭐⭐ | 三方协议集成（BTC + 现货 + Predict） |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
