# Vol-Arb Bot: Predict ↔ Polymarket / 波动率套利机器人

> 反推 Predict 隐含波动率与 Polymarket 比较，超过阈值时交易价差

## 解决什么问题

不同预测市场平台对同一标的的定价效率不同，存在跨平台波动率套利机会。但目前没有自动化工具同时监控 Predict 和 Polymarket 的定价差异。Vol-Arb Bot 实时反推两个平台的隐含波动率，当价差超过阈值时自动执行套利交易。

## 核心功能

- 从 OracleSVI 反推 Predict 的隐含波动率曲线
- 通过 Polymarket API 获取 BTC 期权市场数据，反推 IV
- 实时比较两个平台的波动率微笑曲线
- 价差超过阈值时自动在低价平台买入、高价平台卖出

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::mint | 在 Predict 上执行套利交易 |
| predict::redeem_permissionless | 套利仓位到期后赎回 |
| PredictManager | 管理套利仓位 |
| OracleSVI | 核心数据源，反推 Predict 隐含波动率 |

## 实现方案

### 智能合约 (Move)
- `vol_arb_vault` 模块：套利资金管理
- `position_tracker` 模块：记录跨平台套利仓位
- 关键数据结构：`ArbPosition`（平台 A/B 仓位、入场 IV 差、预期利润）

### Bot / Keeper / 服务端
- IV 计算引擎：从 SVI 参数反推各行权价 IV
- Polymarket 数据采集器：通过 API 获取实时定价
- 套利信号生成器：比较两个平台 IV，生成交易信号
- 双平台执行器：同时提交 Predict 和 Polymarket 交易
- Kelly 仓位管理：根据胜率计算最优仓位

### 前端 / 后端
- 技术栈：React + D3.js + Python FastAPI
- 实时 IV 对比图表（Predict vs Polymarket 微笑曲线）
- 套利机会面板和历史交易记录
- PnL 追踪和风险指标

## 技术架构

Python 后端持续采集两个平台数据 → IV 计算引擎反推波动率 → 信号生成器发现套利机会 → 执行器在 Predict（predict::mint）和 Polymarket 同时下单 → 到期时 redeem → 前端实时展示 IV 对比和套利状态。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 简单的资金管理 |
| Bot/算法 | ⭐⭐⭐⭐⭐ | IV 反推和跨平台套利逻辑复杂 |
| 前端 | ⭐⭐⭐ | IV 曲线对比可视化 |
| 集成复杂度 | ⭐⭐⭐⭐ | 需要集成两个独立平台 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
