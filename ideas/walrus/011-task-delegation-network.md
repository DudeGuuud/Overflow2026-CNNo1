# Task Delegation Agent Network / 任务委派智能体网络

> 主 Agent 拆解复杂任务，委派给专业子 Agent 执行，中间状态全部存入 Walrus / Master Agent decomposes complex tasks, delegates to specialized sub-Agents with all intermediate states stored in Walrus

## 解决什么问题

复杂任务（如完整的市场分析报告、端到端的 DeFi 操作）通常涉及多个专业领域。单个 Agent 难以在所有方面都表现优异。需要一种主-子 Agent 架构，由主 Agent 拆解任务并委派给专业子 Agent，所有中间状态和结果存入 Walrus 确保可追溯。

## 核心功能

- 智能任务分解：主 Agent 将复杂任务拆解为可并行/串行执行的子任务
- 专业子 Agent 池：预置多种专业 Agent（数据收集、分析、写作、交易、审计等）
- 中间状态持久化：每个子任务的输入、输出、状态变更都存入 Walrus
- 任务流可视化：实时展示任务分解树和各子 Agent 执行状态

## 使用的 Walrus / Sui 能力

| 能力 | 用途 |
|------|------|
| Walrus Blob 存储 | 存储任务定义、子任务输入输出、中间状态快照 |
| MemWal 记忆层 | 主 Agent 记住历史任务分解策略和子 Agent 执行效果 |
| Sui 对象模型 | 链上记录任务 ID、子任务关系、执行状态、结果哈希 |
| Walrus 读取 API | 子 Agent 从 Walrus 读取上游任务的输出作为输入 |

## 实现方案

### Agent / AI 部分
- 使用 LangGraph 构建任务分解和调度引擎
- 主 Agent 使用 GPT-4o 进行任务分解和子 Agent 选择
- 子 Agent 使用不同模型：分析用 Claude 3.5、快速处理用 GPT-4o-mini
- 任务队列使用 Redis + RQ（Redis Queue）管理

### Walrus 集成
- 任务定义以 `tasks/{task_id}/definition.json` 存储
- 子任务输出以 `tasks/{task_id}/outputs/{subtask_id}.json` 存储
- 状态快照以 `tasks/{task_id}/snapshots/{timestamp}.json` 存储
- Sui 链上 TaskGraph 对象维护任务 DAG（有向无环图）关系

### 前端 / 后端
- 前端：React + React Flow，可视化任务分解树和执行状态
- 后端：Python FastAPI + Redis 任务队列
- 关键页面：任务创建页、执行监控页（实时树状图）、结果查看页、Agent 管理页

## 技术架构

用户提交复杂任务 → 主 Agent 分析并分解为子任务 DAG → 子任务写入 Walrus → 分配给对应专业子 Agent → 子 Agent 从 Walrus 读取输入 → 执行后将结果写入 Walrus → 更新 Sui 链上 TaskGraph → 触发下游子任务 → 全部完成后主 Agent 汇总 → 最终结果写入 Walrus。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Walrus 集成 | ⭐⭐⭐ | 高频中间状态写入需要优化 |
| AI/Agent | ⭐⭐⭐⭐⭐ | 任务分解、Agent 调度、异常处理 |
| 前端 | ⭐⭐⭐ | 任务树可视化和实时状态更新 |
| 集成复杂度 | ⭐⭐⭐⭐⭐ | 任务 DAG 调度 + 多 Agent 协调 + 状态同步 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
