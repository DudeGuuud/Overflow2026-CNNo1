# Agent Identity System / Agent 身份与声誉系统

> 基于 Sui 对象为 AI Agent 建立链上身份、能力证明和声誉评分

## 解决什么问题

AI Agent 在链上的活动缺乏标准化的身份标识和声誉系统，用户无法评估 Agent 的可信度和能力水平。恶意 Agent 可以轻易更换地址重新出现，链上生态缺乏对 Agent 行为的追溯和评估机制。本项目基于 Sui 对象模型为 AI Agent 建立链上身份，记录能力证明、历史行为和声誉评分，让 Agent 拥有可验证的链上信誉，促进可信 Agent 生态的发展。

## 核心功能

- Agent 身份注册：为 AI Agent 创建链上身份对象，声明能力、所有者和服务范围
- 声誉评分系统：根据 Agent 的链上行为（任务完成率、用户评价、异常事件）动态更新声誉分
- 能力证明机制：Agent 通过完成链上挑战或提交工作证明获得能力徽章

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `AgentIdentity` 对象存储 Agent 身份、能力和声誉数据，归属 Agent 所有者 |
| 动态字段 | 为身份对象附加能力证明、服务记录和评价数据 |
| NFT | 能力徽章以 NFT 形式颁发，可验证且不可伪造 |
| zkLogin | Agent 可通过 zkLogin 关联社交身份，增强身份可信度 |

## 实现方案

### 智能合约 (Move)
- **`agent_identity` 模块**：定义 `AgentIdentity` 对象，字段包括 `agent_address: address`、`name: String`、`description: String`、`capabilities: vector<u8>`、`reputation_score: u64`、`task_count: u64`、`success_rate: u64`、`created_at: u64`、`owner: address`
- **`reputation` 模块**：`update_reputation(identity: &mut AgentIdentity, delta: u64, positive: bool, evidence: vector<u8>)` 更新声誉
- **`capability_badge` 模块**：`mint_badge(identity: &AgentIdentity, capability: u8, level: u8): CapabilityBadge` 铸造能力徽章
- 关键数据结构：`ServiceRecord { service_type: u8, success: bool, rating: u64, reviewer: address, timestamp: u64 }`

### 前端 / 后端
- **前端**：React + Next.js，Agent 身份卡片、声誉仪表盘、能力徽章展示、服务市场
- **后端**：Python Agent 服务，声誉计算引擎、能力验证器、身份索引服务
- 关键功能：身份注册、声誉查询、徽章管理、Agent 搜索、评价提交

### AI / Agent 部分
- 声誉评估：多因子模型综合评估 Agent 的任务完成率、响应速度、用户满意度和异常行为
- 能力验证：AI 设计和评估链上挑战任务，验证 Agent 声称的能力是否属实
- 异常检测：监控 Agent 行为模式，检测异常操作（如频繁失败、异常资金流向）
- Agent 工作流：身份注册 → 能力声明 → 挑战验证 → 服务执行 → 评价收集 → 声誉更新

## 技术架构

Agent 所有者为 Agent 注册 `AgentIdentity` Move 对象，声明能力范围。能力验证器生成链上挑战任务，Agent 完成后获得能力徽章 NFT。Agent 在链上提供服务时，用户可提交评价和打分。声誉计算引擎定期聚合评价数据和行为指标，通过 PTB 更新 `AgentIdentity` 的声誉评分。异常检测器监控 Agent 行为，恶意行为触发声誉惩罚。前端提供 Agent 目录和详情页。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 身份管理、声誉更新和徽章铸造 |
| 前端 | ⭐⭐⭐ | 身份卡片、声誉面板和服务市场 |
| AI/ML | ⭐⭐⭐ | 声誉评估、能力验证和异常检测 |
| 集成复杂度 | ⭐⭐⭐ | 身份索引、评价系统和行为监控 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
