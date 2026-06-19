# DeFi Risk Sentry / DeFi 风险哨兵

> 接入 Sui 借贷协议预言机，AI 模型实时计算风险评分，超阈值自动通过 Move 策略对象暂停市场

## 解决什么问题

DeFi 协议在极端行情下缺乏快速、自动化的风险响应机制。传统方案依赖人工监控，响应速度慢，常导致大规模清算和资金损失。本项目利用 AI 模型实时评估链上风险指标，并通过 Sui Move 对象模型实现链上自动化风险控制，确保协议在危急时刻能毫秒级响应。

## 核心功能

- 实时抓取 Scallop / Navi 等借贷协议的预言机价格和清算数据，AI 模型计算综合风险评分（0-100）
- 风险评分超过阈值时，自动通过 Move 策略对象触发市场暂停交易（`market_pause` flag）
- DAO 治理可覆盖/调整 AI 决策，所有操作链上可审计
- 历史风险事件回溯分析，生成风险报告

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | 风险策略对象 `RiskPolicy` 存储。阈值、白名单、暂停状态全部以共享对象形式上链 |
| PTB | 将「读取预言机 → 计算风险 → 判断阈值 → 暂停市场」打包为原子化交易 |
| 共享对象 | `MarketStatus` 共享对象允许多方（Agent / DAO）竞争性写入暂停状态 |
| 事件（Event） | 发出 `RiskAlertEvent` 供前端和外部系统订阅告警 |

## 实现方案

### 智能合约 (Move)
- **`risk_policy` 模块**：定义 `RiskPolicy` 对象，字段包括 `threshold: u64`、`paused: bool`、`dao_override: bool`、`last_check_epoch: u64`
- **`market_guard` 模块**：`pause_market(policy: &mut RiskPolicy, market_id: ID)` 和 `resume_market()` 函数，检查调用者是否为授权 Agent 地址
- **`oracle_reader` 模块**：通过 PTB 调用 Scallop/Naviprotocol 的预言机接口读取价格，将结果传入风险判断
- 关键数据结构：`RiskScore { score: u64, timestamp: u64, source: String }`

### 前端 / 后端
- **前端**：React + Next.js + TailwindCSS，展示实时风险仪表盘、历史图表、DAO 投票面板
- **后端**：Rust/TypeScript Agent 服务，WebSocket 订阅链上事件，调用 OpenAI API 做风险分析
- 关键页面：风险总览 Dashboard、协议详情页、告警历史、DAO 治理页

### AI / Agent 部分
- 使用 OpenAI GPT-4o 分析多维风险指标（价格波动率、清算量、流动性深度）
- Agent 工作流：每 30 秒轮询预言机数据 → 输入 XGBoost 风险模型计算评分 → 超阈值时构造 PTB 提交链上
- LangChain 编排多步骤决策链：数据获取 → 风险建模 → 决策 → 链上执行

## 技术架构

Agent 服务通过 Sui RPC 订阅预言机价格变更事件，将多维数据输入 AI 风险模型（XGBoost + GPT-4o 辅助判断）。模型输出风险评分后，若超阈值则构造 PTB（读取预言机 → 校验评分 → 调用 `pause_market`）。Move 合约中 `RiskPolicy` 共享对象记录策略状态，DAO 多签可覆盖暂停决策。前端通过 WebSocket 实时展示风险等级和告警。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 需要设计共享对象权限模型和预言机交互 |
| 前端 | ⭐⭐ | 仪表盘 + 图表，常规 React 开发 |
| AI/ML | ⭐⭐⭐⭐ | 需要训练/微调风险模型，多维指标融合 |
| 集成复杂度 | ⭐⭐⭐ | 需对接 Scallop/Naviprotocol 预言机和 PTB |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
