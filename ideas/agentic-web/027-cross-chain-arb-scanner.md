# Cross-Chain Arbitrage Scanner / 跨链套利机会扫描

> AI 监控 Sui 与其他链的价差，发现套利机会时生成 PTB 策略

## 解决什么问题

加密资产在不同链上的 DEX 存在持续的价格偏差，但手动发现和执行跨链套利效率极低，机会转瞬即逝。传统套利工具缺乏 AI 驱动的智能分析，无法综合评估价差、Gas 成本、桥接费用和滑点。本项目利用 AI 实时扫描 Sui 与 Ethereum、Solana 等链上的价差，发现机会时自动生成 PTB 执行策略，通过原子化交易捕获套利利润。

## 核心功能

- 实时监控 Sui（DeepBook/Cetus）与其他主流链 DEX 间的代币价差，AI 评估净套利收益
- 发现机会时自动构造 PTB 策略（桥接 + 兑换 + 利润归集），用户确认后一键执行
- 历史套利机会回溯分析，AI 生成最优路径推荐和风险提示

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| PTB | 将「桥接资产 → DEX 兑换 → 利润归集 → 策略对象更新」打包为原子化交易 |
| DeepBook | 作为 Sui 端的流动性源，执行现货兑换获取价差收益 |
| Move 对象模型 | `ArbStrategy` 对象存储套利策略配置、历史执行记录和累计收益 |
| 共享对象 | `PriceSnapshot` 共享对象记录多链价格快照，供 Agent 竞争性读取 |

## 实现方案

### 智能合约 (Move)
- **`arb_strategy` 模块**：定义 `ArbStrategy` 对象，字段包括 `owner: address`、`max_position: u64`、`min_profit_bps: u64`、`exec_count: u64`、`total_profit: u256`、`active: bool`
- **`price_snapshot` 模块**：`record_price(snapshot: &mut PriceSnapshot, chain_id: u8, token: String, price: u64, timestamp: u64)` 记录跨链价格
- **`arb_executor` 模块**：`execute_arb(strategy: &mut ArbStrategy, route: vector<RouteStep>)` 验证并执行套利路径
- 关键数据结构：`RouteStep { protocol: String, action: u8, token_in: ID, token_out: ID, amount: u64 }`

### 前端 / 后端
- **前端**：React + Next.js + TradingView 图表，展示跨链价差热力图、套利机会列表、历史收益
- **后端**：Rust Agent 服务，WebSocket 订阅多链价格，计算套利机会并构造 PTB
- 关键功能：多链价格面板、一键执行、收益追踪、风险限额设置

### AI / Agent 部分
- 价差检测：实时比较 Sui DeepBook/Cetus 与 Uniswap/Raydium 的代币对价格
- 利润评估：综合考量 Gas 费、桥接费用、滑点、价格影响，AI 计算净利润
- 路径优化：强化学习模型选择最优桥接路径和兑换顺序
- Agent 工作流：多链价格采集 → 价差检测 → 利润评估 → PTB 生成 → 用户确认 → 链上执行

## 技术架构

Agent 服务通过 WebSocket 同时订阅 Sui（DeepBook/Cetus）和外部链（Ethereum Uniswap、Solana Raydium）的价格数据。价格比较引擎实时计算各交易对的跨链价差，扣除 Gas、桥接费和滑点后筛选净利润为正的机会。RL 路径优化器选择最优执行路径，构造包含桥接和兑换步骤的 PTB。用户在前端确认后一键提交，`ArbStrategy` Move 对象记录执行结果和累计收益。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 策略对象管理、路由执行和利润结算 |
| 前端 | ⭐⭐⭐ | 多链价格热力图、套利面板和实时数据展示 |
| AI/ML | ⭐⭐⭐ | 路径优化和利润预测模型 |
| 集成复杂度 | ⭐⭐⭐⭐⭐ | 需对接多链 RPC、跨链桥和多个 DEX |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
