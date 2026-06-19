# DeFi Yield Leaderboard / DeFi 收益排行榜

> 聚合 Sui 生态所有收益池，AI 预测未来 APY 趋势，推荐最优策略

## 解决什么问题

Sui 生态 DeFi 协议数量增长迅速，收益池分散在 Scallop、Navi、Aftermath、Turbbos 等多个协议中。用户需要在多个界面间切换比较 APY，且当前 APY 不能反映未来趋势。本项目聚合 Sui 生态所有收益池数据，AI 预测未来 APY 走势并综合风险评估，为用户推荐最优策略，同时提供链上可验证的历史收益记录。

## 核心功能

- 自动聚合 Sui 生态所有 DeFi 协议的收益池数据（APY、TVL、风险等级）
- AI 时序模型预测未来 7 天 APY 走势，标注上升/下降趋势
- 综合评分：结合 APY、TVL 稳定性、协议安全性和流动性给出推荐排序
- 一键跳转：从排行榜直接跳转到协议存入页面，或通过 PTB 直接存入

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `YieldSnapshot` 对象存储收益池快照和预测数据 |
| PTB | 一键存入功能：从排行榜直接构造 PTB 跳转到目标协议存入 |
| 链上数据 | 直接读取协议共享对象获取实时 TVL 和收益数据 |
| 动态字段 | 为每个收益池附加历史数据和预测指标 |

## 实现方案

### 智能合约 (Move)
- **`yield_snapshot` 模块**：定义 `YieldSnapshot` 对象，字段包括 `pool_id: ID`、`protocol: String`、`apy_current: u64`、`apy_predicted_7d: u64`、`tvl: u256`、`risk_score: u64`、`composite_score: u64`、`timestamp: u64`
- **`yield_registry` 模块**：`update_snapshot(registry: &mut YieldRegistry, snapshot: YieldSnapshot)` 批量更新收益池数据
- **`quick_deposit` 模块**：`deposit_from_ranking(user_coin: Coin, pool_id: ID, min_amount_out: u64)` 从排行榜一键存入目标池
- 关键数据结构：`PoolMetrics { pool_id: ID, apy_7d_avg: u64, apy_30d_avg: u64, tvl_change_24h: i256, risk_tier: u8 }`

### 前端 / 后端
- **前端**：React + TailwindCSS，收益池排行表格、APY 趋势迷你图、协议对比筛选器
- **后端**：TypeScript Agent 服务，多协议数据聚合、APY 预测模型、评分引擎
- 关键功能：排序筛选、趋势图表、一键存入

### AI / Agent 部分
- APY 预测：Prophet + LSTM 混合模型预测 7 天 APY 走势
- 风险评估：多因子模型评估协议风险（合约审计状态、TVL 波动性、历史漏洞）
- 综合评分：加权计算 APY 预期收益、风险调整收益和流动性评分
- Agent 工作流：每小时抓取协议数据 → 计算 APY → 运行预测模型 → 更新评分 → 链上快照更新

## 技术架构

Agent 服务每小时通过 Sui RPC 和各协议索引器聚合所有收益池的实时数据（APY、TVL、用户数）。原始数据经清洗后输入 APY 预测模型（Prophet 季节性分解 + LSTM 趋势预测），同时风险模型评估协议安全性。综合评分引擎加权所有维度输出推荐排序，结果通过 PTB 批量更新 YieldSnapshot 链上对象。前端展示排行表格并支持一键通过 PTB 直接存入目标池。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 收益快照存储和一键存入逻辑 |
| 前端 | ⭐⭐⭐ | 排行表格、筛选和趋势图表 |
| AI/ML | ⭐⭐⭐ | APY 时序预测和多因子评分模型 |
| 集成复杂度 | ⭐⭐⭐⭐ | 需对接 Sui 生态所有主要 DeFi 协议 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
