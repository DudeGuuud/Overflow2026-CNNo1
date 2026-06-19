# Personal Finance AI Assistant / 个人财务 AI 助手

> 接入用户 Sui 钱包，AI 分析资产分布、收益情况，自动推荐和执行优化策略

## 解决什么问题

DeFi 用户管理分散在多个协议中的资产，需要手动追踪收益、比较利率和执行再平衡，操作繁琐且容易错过最优时机。缺乏一站式的链上财务管理工具，用户难以全面了解资产状况和优化空间。本项目接入用户 Sui 钱包，AI 全面分析资产分布和收益表现，自动推荐优化策略（如迁移到高 APY 池、复投收益），用户确认后一键通过 PTB 执行。

## 核心功能

- 全景资产视图：聚合用户在 Sui 各协议的资产（存款、借款、LP、质押），展示净值和收益趋势
- AI 优化推荐：分析当前资产配置的收益率和风险，推荐具体的优化操作（如换池、复投、再平衡）
- 一键执行：用户确认推荐策略后，通过 PTB 原子化执行多步操作

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `FinanceProfile` 对象存储用户的财务配置偏好和策略执行历史 |
| PTB | 将「提取资产 → 兑换 → 存入新池 → 复投收益」等多步操作打包为原子交易 |
| DeepBook | 执行资产兑换操作，获取最优兑换路径 |
| 链上数据 | 读取用户在各协议的持仓和收益数据，构建完整的资产视图 |

## 实现方案

### 智能合约 (Move)
- **`finance_profile` 模块**：定义 `FinanceProfile` 对象，字段包括 `owner: address`、`risk_tolerance: u8`、`target_allocation: vector<u8>`、`rebalance_threshold: u64`、`auto_compound: bool`、`total_optimizations: u64`、`total_saved: u256`、`created_at: u64`
- **`strategy_log` 模块**：`log_strategy(profile: &mut FinanceProfile, strategy_type: u8, tokens: vector<String>, estimated_apr: u64, gas_cost: u64)` 记录执行策略
- **`rebalance_trigger` 模块**：`check_rebalance(profile: &FinanceProfile, current_allocation: vector<u8>): bool` 检查是否需要再平衡
- 关键数据结构：`StrategyRecord { strategy_type: u8, input_tokens: vector<String>, output_tokens: vector<String>, estimated_apr: u64, executed_at: u64 }`

### 前端 / 后端
- **前端**：React + ECharts/Recharts，资产总览仪表盘、收益趋势图、优化推荐卡片、执行历史
- **后端**：Python Agent 服务，资产聚合器、收益分析引擎、策略优化器、执行引擎
- 关键功能：钱包连接、资产概览、收益分析、优化推荐、一键执行、历史记录

### AI / Agent 部分
- 资产分析：AI 评估用户当前资产配置的效率，识别低收益和过度集中的仓位
- 策略生成：GPT-4o 根据用户风险偏好和市场状况生成个性化优化策略
- 收益预测：基于历史 APY 数据和市场趋势预测各策略的预期收益
- Agent 工作流：资产聚合 → 配置分析 → 策略生成 → 收益预测 → 用户确认 → PTB 执行 → 结果记录

## 技术架构

用户连接 Sui 钱包后，资产聚合器通过 RPC 扫描所有协议合约，汇总用户的存款、借款、LP 仓位和质押资产。AI 分析引擎评估当前配置的收益率和风险水平，识别优化机会。策略优化器生成具体的操作建议（如「从 Scallop 迁移 1000 USDC 到 Navi，预计 APR 提升 2.1%」）。用户确认后构造 PTB 原子化执行所有步骤。执行结果记录在 `FinanceProfile` 对象中。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 财务配置和策略日志管理 |
| 前端 | ⭐⭐⭐ | 资产仪表盘、收益图表和推荐面板 |
| AI/ML | ⭐⭐⭐ | 资产分析、策略优化和收益预测 |
| 集成复杂度 | ⭐⭐⭐⭐ | 多协议资产聚合和跨协议 PTB 编排 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
