# Multi-Agent Collaboration Framework / 多 Agent 协作框架

> 在 Sui 上构建多 Agent 协调系统，Agent 间通过 Move 对象传递任务和状态

## 解决什么问题

单个 AI Agent 的能力有限，复杂的链上任务需要多个专业 Agent 协作完成。现有 Agent 框架缺乏链上原生的协作机制，Agent 间的任务分发、状态同步和结果聚合依赖中心化服务。本项目在 Sui 上构建多 Agent 协作框架，利用 Move 对象模型实现 Agent 间的任务传递、状态同步和激励分配，让多个专业 Agent 以去中心化方式协同完成复杂 DeFi 操作。

## 核心功能

- 任务编排引擎：将复杂任务拆解为子任务，分配给不同专业 Agent（分析型、交易型、监控型）
- Move 对象消息传递：Agent 间通过链上 Move 对象传递任务参数、中间结果和状态确认
- 激励分配：任务完成后根据各 Agent 的贡献自动分配激励，机制链上透明

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `Task`、`AgentRegistry`、`TaskResult` 对象管理任务生命周期和 Agent 注册 |
| 共享对象 | `TaskBoard` 共享对象作为任务看板，所有 Agent 竞争性认领和提交任务 |
| PTB | 将「任务创建 → 子任务分发 → 结果收集 → 激励分配」编排为原子化流程 |
| 事件（Event） | 发出 `TaskCreatedEvent`、`TaskClaimedEvent`、`TaskCompletedEvent` 驱动协作流程 |

## 实现方案

### 智能合约 (Move)
- **`agent_registry` 模块**：定义 `AgentProfile` 对象，字段包括 `agent_id: address`、`capabilities: vector<u8>`、`reputation: u64`、`tasks_completed: u64`、`stake: Balance<SUI>`、`registered_at: u64`
- **`task_board` 模块**：定义 `TaskBoard` 共享对象，`create_task(board: &mut TaskBoard, description: vector<u8>, reward: Balance<SUI>, deadline: u64)` 和 `claim_task()` 管理任务
- **`task_result` 模块**：`submit_result(board: &mut TaskBoard, task_id: ID, result_data: vector<u8>, agent: address)` 提交结果
- 关键数据结构：`Task { task_id: ID, parent_id: Option<ID>, description: vector<u8>, status: u8, assignee: Option<address>, reward: Balance<SUI>, deadline: u64 }`

### 前端 / 后端
- **前端**：React + React Flow，任务流可视化、Agent 状态面板、协作拓扑图、激励看板
- **后端**：Rust/Python Agent 框架，任务编排器、调度器、Agent 运行时、消息总线
- 关键功能：任务创建、Agent 注册、协作可视化、结果验证、激励分配

### AI / Agent 部分
- 任务拆解：GPT-4o 将用户的复杂需求拆解为可独立执行的子任务序列
- Agent 调度：根据 Agent 能力画像和声誉评分匹配最适合的 Agent 执行子任务
- 结果验证：AI 交叉验证多个 Agent 的执行结果，检测异常和冲突
- Agent 工作流：用户请求 → 任务拆解 → 链上注册 → Agent 认领 → 执行 → 提交结果 → 验证 → 激励分配

## 技术架构

用户提交复杂任务（如「分析市场 → 制定策略 → 执行交易 → 监控持仓」），编排引擎通过 GPT-4o 拆解为子任务并注册到 `TaskBoard` 共享对象。各专业 Agent 监听看板事件，认领匹配自身能力的子任务。Agent 执行完毕后将结果写入 `TaskResult` 对象。协调 Agent 收集所有子任务结果，验证一致性后触发激励分配 PTB。前端展示实时的协作拓扑和任务进度。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐⭐ | 任务看板、Agent 注册和激励分配机制 |
| 前端 | ⭐⭐⭐ | 协作拓扑可视化和任务面板 |
| AI/ML | ⭐⭐⭐ | 任务拆解、Agent 调度和结果验证 |
| 集成复杂度 | ⭐⭐⭐⭐⭐ | 多 Agent 运行时、并发控制和状态同步 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
