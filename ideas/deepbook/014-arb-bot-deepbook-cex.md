# Arbitrage Bot: DeepBook ↔ CEX / 套利机器人

> 监控 DeepBook 与 Binance 价差，实时套利

## 解决什么问题

链上 DEX 和中心化交易所之间存在价格差异，但套利需要低延迟的监控和执行系统。套利机器人实时比较 DeepBook 和 Binance 的 BTC/dUSDC 价格，当价差超过交易成本时自动在两个平台对向交易，锁定无风险利润。

## 核心功能

- WebSocket 实时监控 DeepBook 和 Binance 的 BTC/dUSDC 订单簿
- 价差检测：当价差超过 Gas + 滑点阈值时触发
- 双向执行：同时在 DeepBook 和 Binance 对向下单
- 风险管理：最大单笔暴露、日累计限额、异常熔断

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| deepbook 现货 | 在 DeepBook 上执行买卖 |
| predict::mint | 套利利润可投入 Predict 策略增值 |
| predict::redeem_permissionless | Predict 策略收益赎回 |
| dUSDC | 作为报价资产的桥梁 |

## 实现方案

### 智能合约 (Move)
- `arb_vault` 模块：管理套利资金和收益
- 关键数据结构：`ArbState`（链上余额、累计套利次数、累计利润）

### Bot / Keeper / 服务端
- 行情采集器：同时订阅 DeepBook 和 Binance WebSocket
- 价差分析器：实时计算有效价差（扣除手续费和 Gas）
- 执行引擎：构建 PTB 在 DeepBook 下单 + Binance API 下单
- 资金平衡器：确保两个平台有足够资金执行套利
- 监控告警：异常价差检测、资金不足预警

### 前端 / 后端
- 技术栈：React + WebSocket + Python
- 实时价差图表（DeepBook vs Binance）
- 套利交易历史和利润统计
- 资金分布面板（两个平台的余额）

## 技术架构

Python Bot 同时连接 DeepBook 索引器和 Binance WebSocket → 实时比较价格 → 发现套利机会时构建 PTB → DeepBook 交易通过 Sui 链提交，Binance 交易通过 API 提交 → 两笔交易几乎同时执行 → 利润累积到 Vault。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 基础资金管理 |
| Bot/算法 | ⭐⭐⭐⭐ | 低延迟价差检测和双向执行 |
| 前端 | ⭐⭐ | 监控面板 |
| 集成复杂度 | ⭐⭐⭐⭐ | 链上链下双平台联动 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
