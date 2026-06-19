# AI NPC Economy Agent / AI NPC 经济 Agent

> 游戏中的 AI NPC 在 Sui 上自主交易资源，影响游戏经济系统

## 解决什么问题

链游经济系统面临流动性不足和玩家行为不可预测的挑战，NPC 商人通常使用固定脚本，无法根据市场变化动态调整策略。这导致游戏内经济缺乏活力和深度。本项目让 AI NPC 成为 Sui 链上的自主经济 Agent，拥有独立钱包和交易策略，在游戏资产市场上自主买卖资源，为链游经济提供流动性和价格发现功能，创造更真实和动态的虚拟经济。

## 核心功能

- AI NPC 拥有独立 Sui 钱包和资源库存，根据市场状况自主决定买卖策略
- NPC 之间和 NPC 与玩家之间在去中心化市场上自由交易，形成动态价格
- 游戏开发者可配置 NPC 行为参数（风险偏好、库存上限、交易频率）

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `NPCAgent` 对象存储 NPC 身份、库存、资金和策略参数 |
| PTB | NPC 执行交易时构造 PTB（检查库存 → 计算价格 → 执行兑换 → 更新状态） |
| 共享对象 | `GameMarket` 共享对象作为去中心化交易市场，NPC 和玩家均可挂单 |
| 事件（Event） | 发出 `NPCTradeEvent` 记录 NPC 交易行为，供游戏引擎和经济分析使用 |

## 实现方案

### 智能合约 (Move)
- **`npc_agent` 模块**：定义 `NPCAgent` 对象，字段包括 `npc_id: ID`、`name: String`、`personality: u8`、`balance: Balance<SUI>`、`inventory: Bag`、`strategy_hash: vector<u8>`、`trade_count: u64`、`created_at: u64`
- **`game_market` 模块**：`place_order(market: &mut GameMarket, seller: address, item: ID, price: u64)` 和 `fill_order()` 管理挂单
- **`npc_strategy` 模块**：`update_strategy(npc: &mut NPCAgent, new_strategy: vector<u8>)` 更新 NPC 策略参数
- 关键数据结构：`MarketOrder { order_id: ID, seller: address, item_type: u8, quantity: u64, price: u64 }`

### 前端 / 后端
- **前端**：React + Three.js，游戏世界可视化、NPC 状态面板、市场行情图表、交易历史
- **后端**：Python Agent 服务，NPC AI 引擎、市场模拟器、策略优化器
- 关键功能：NPC 监控面板、市场行情、交易回放、NPC 行为调参、经济指标

### AI / Agent 部分
- 决策引擎：强化学习模型根据库存水平、市场价格和供需关系生成交易决策
- 策略多样化：不同 NPC 有不同风险偏好和交易风格（保守型、激进型、套利型）
- 市场影响：NPC 交易行为影响游戏内资源价格，形成动态经济平衡
- Agent 工作流：市场扫描 → 策略计算 → 决策生成 → PTB 构造 → 链上执行 → 状态更新

## 技术架构

每个 AI NPC 对应一个 `NPCAgent` Move 对象和独立的 Agent 进程。Agent 进程定期扫描 `GameMarket` 共享对象上的挂单，强化学习模型根据 NPC 库存、市场供需和个性化参数生成交易决策。决策转化为 PTB 提交链上执行，原子化完成资产交换。NPC 交易数据反馈至模型进行在线学习。前端可视化展示 NPC 行为和游戏经济态势。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | NPC 对象、市场合约和资产交换逻辑 |
| 前端 | ⭐⭐⭐ | 游戏世界可视化和经济面板 |
| AI/ML | ⭐⭐⭐⭐ | 强化学习 NPC 决策模型和策略多样化 |
| 集成复杂度 | ⭐⭐⭐⭐ | 多 NPC 并发管理和游戏引擎集成 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
