# On-chain Will / Condition Trigger / 链上遗嘱/条件触发器

> 用户设定条件（如长期未活跃），AI 监控并自动触发资产转移 PTB

## 解决什么问题

加密资产持有者面临一个独特问题：如果发生意外，链上资产可能永久丢失，因为没有传统银行的遗产继承机制。现有的多签方案需要受益人主动操作，且缺乏条件自动检测能力。本项目利用 AI Agent 持续监控用户的链上活跃状态，当触发预设条件（如长期未活跃、特定日期到达）时，自动执行资产转移 PTB，实现链上遗嘱和条件触发功能。

## 核心功能

- 创建链上遗嘱：设定触发条件（N 天未活跃、指定日期、链上事件触发）和受益人地址
- AI Agent 持续监控创建者的链上活跃状态（交易频率、登录行为）
- 条件触发后自动构造 PTB 执行资产转移，支持多受益人按比例分配
- 时间锁和取消机制：创建者随时可取消或修改，误触发有缓冲期

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `OnchainWill` 对象存储条件、受益人和资产锁定状态 |
| PTB | 条件触发时原子化执行「解锁资产 → 按比例分配 → 转账到受益人」 |
| 时间锁 | 内置时间锁机制确保缓冲期内可取消 |
| 事件（Event） | 发出 `WillActivatedEvent` 和 `WillExecutedEvent` |

## 实现方案

### 智能合约 (Move)
- **`onchain_will` 模块**：定义 `OnchainWill` 对象，字段包括 `creator: address`、`conditions: vector<Condition>`、`beneficiaries: vector<Beneficiary>`、`locked_assets: Bag`、`status: u8`（active/triggered/executed/cancelled）、`created_at: u64`、`buffer_period_epochs: u64`
- **`condition` 模块**：定义条件类型 `Condition` — `InactivityDays(u64)`、`DeadlineEpoch(u64)`、`CustomEvent(ID)`
- **`will_executor` 模块**：`trigger_will(will: &mut OnchainWill, proof: ConditionProof)` 验证条件后进入缓冲期，`execute_will(will: &mut OnchainWill)` 缓冲期后执行分配
- 关键数据结构：`Beneficiary { addr: address, share_bp: u64 }`、`ConditionProof { condition_index: u8, evidence: vector<u8> }`

### 前端 / 后端
- **前端**：React + Next.js，遗嘱创建向导、条件配置、资产锁定状态展示、受益人管理
- **后端**：TypeScript Agent 服务，活跃度监控、条件评估、触发执行
- 关键功能：遗嘱仪表盘、活跃度日历、触发通知

### AI / Agent 部分
- 活跃度监控：Agent 定期检查创建者地址的交易活动，更新最后活跃时间戳
- 智能条件评估：AI 综合多维度信号判断是否应触发（不仅仅是交易频率，还包括协议交互、签名消息等）
- 防误触发：LLM 分析链上行为模式，区分"真正不活跃"和"仅是持有资产"
- Agent 工作流：每日扫描所有活跃遗嘱 → 检查条件 → 超阈值触发 → 进入缓冲期 → 等待后执行分配

## 技术架构

用户创建 OnchainWill 对象，设定触发条件、受益人分配比例，并将资产存入锁定。Agent 服务每日扫描所有活跃遗嘱，检查创建者的链上活动记录。当检测到不活跃天数超过设定阈值（或其他条件满足）时，提交 ConditionProof 触发遗嘱进入缓冲期。缓冲期（如 7 天）内创建者可登录取消。缓冲期结束后 Agent 构造 PTB（解锁资产 → 按比例拆分 → 转账到各受益人），原子化执行。所有状态变更通过事件上链。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐⭐ | 资产锁定、条件验证和分配逻辑设计 |
| 前端 | ⭐⭐⭐ | 遗嘱管理和状态展示 |
| AI/ML | ⭐⭐ | 活跃度分析和条件评估 |
| 集成复杂度 | ⭐⭐⭐ | 多维度活跃度监控和触发可靠性保障 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
