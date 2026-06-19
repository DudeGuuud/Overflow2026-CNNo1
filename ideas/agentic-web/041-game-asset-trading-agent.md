# Game Asset Trading Agent / 游戏资产自动交易 Agent

> 玩家设定买卖规则，Agent 在游戏资产市场上自动执行

## 解决什么问题

链游玩家需要时刻盯盘才能在游戏资产市场上获得有利交易，但这与游戏体验本身矛盾。手动交易容易因情绪化决策导致损失，且玩家无法 24 小时监控市场。本项目让玩家通过自然语言设定游戏资产的买卖规则，AI Agent 在去中心化游戏资产市场上 24/7 自动执行交易，支持限价单、条件触发和组合策略，让玩家专注于游戏本身。

## 核心功能

- 自然语言设定交易规则：如「当传奇之剑地板价低于 50 SUI 时买入，持有至价格翻倍后卖出」
- 多策略组合：支持同时运行多个交易策略，每个策略有独立预算和止损线
- 交易回测：AI 根据历史市场数据模拟策略执行效果，展示预期收益和风险

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `TradingRule` 对象存储玩家设定的交易策略，预算和止损参数 |
| PTB | Agent 执行交易时构造 PTB（检查预算 → 匹配挂单 → 资产交换 → 更新规则状态） |
| 共享对象 | `AssetMarket` 共享对象作为去中心化游戏资产交易市场 |
| 事件（Event） | 发出 `AutoTradeEvent` 记录 Agent 自动执行的每笔交易详情 |

## 实现方案

### 智能合约 (Move)
- **`trading_rule` 模块**：定义 `TradingRule` 对象，字段包括 `owner: address`、`asset_type: u8`、`action: u8`（Buy/Sell）、`target_price: u64`、`quantity: u64`、`budget: Balance<SUI>`、`stop_loss: u64`、`active: bool`、`exec_count: u64`
- **`asset_market` 模块**：`list_asset(market: &mut AssetMarket, seller: address, asset: GameAsset, price: u64)` 和 `buy_asset()` 管理挂单
- **`rule_executor` 模块**：`execute_rule(rule: &mut TradingRule, market: &mut AssetMarket, execution_price: u64)` 执行规则
- 关键数据结构：`ExecutionRecord { rule_id: ID, action: u8, price: u64, quantity: u64, profit: u256, timestamp: u64 }`

### 前端 / 后端
- **前端**：React + TradingView 组件，策略编辑器、市场行情、持仓面板、收益图表
- **后端**：Python Agent 服务，规则解析器、市场监控器、执行引擎、回测系统
- 关键功能：自然语言规则输入、策略管理、市场看板、交易历史、收益分析

### AI / Agent 部分
- 规则解析：GPT-4o 将自然语言交易描述转换为结构化交易规则（条件、动作、预算、止损）
- 市场监控：实时扫描市场挂单和成交数据，匹配激活条件
- 策略优化：基于历史数据的回测引擎评估策略表现，AI 建议参数调整
- Agent 工作流：规则解析 → 链上注册 → 市场扫描 → 条件匹配 → PTB 构造 → 链上执行 → 结果记录

## 技术架构

玩家通过自然语言描述交易策略，GPT-4o 解析为结构化 `TradingRule` 参数并注册为 Move 对象。Agent 服务 24/7 监控 `AssetMarket` 共享对象的市场数据，当条件匹配时构造 PTB 执行交易。每笔执行结果记录在链上事件中。回测系统加载历史市场数据，模拟策略执行效果。前端提供策略管理、实时市场行情和收益追踪面板。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 交易规则管理、市场挂单和资产交换 |
| 前端 | ⭐⭐⭐ | 策略编辑器、行情面板和收益图表 |
| AI/ML | ⭐⭐⭐ | 规则解析、条件匹配和策略回测 |
| 集成复杂度 | ⭐⭐⭐ | 市场监控、交易执行和状态同步 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
