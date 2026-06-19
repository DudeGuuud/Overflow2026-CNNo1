# Decentralized Content Moderation / 去中心化内容审核 Agent

> 社区提交的内容由 AI 初审，有争议的提交链上投票，结果写入 Move 对象

## 解决什么问题

Web3 社交和内容平台面临内容审核的难题：中心化审核不透明且容易被滥用，完全去中心化又难以应对垃圾信息和有害内容。现有方案缺乏 AI 与链上治理的有效结合。本项目设计双层审核机制：AI Agent 进行初审快速过滤明显违规内容，争议内容提交链上社区投票裁决，审核结果写入 Move 对象确保透明可审计，实现效率与公平的平衡。

## 核心功能

- AI 初审：自动检测垃圾信息、违规内容和恶意链接，明确违规内容直接标记
- 争议裁决：AI 不确定或用户申诉的内容提交链上社区投票，持代币者参与裁决
- 审核透明：所有审核决定（AI 或社区）记录在 Move 对象中，支持回溯审计

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `ModerationRecord` 对象存储每条内容的审核状态、理由和结果 |
| 共享对象 | `VotingSession` 共享对象管理争议内容的投票流程和计票 |
| 事件（Event） | 发出 `ContentFlaggedEvent` 和 `VoteCompletedEvent` 通知审核状态变更 |
| 链上投票 | 持代币者通过 PTB 对争议内容投票，投票权重按持有量计算 |

## 实现方案

### 智能合约 (Move)
- **`moderation_record` 模块**：定义 `ModerationRecord` 对象，字段包括 `content_hash: vector<u8>`、`status: u8`（Pending/Approved/Rejected/Appealed）、`ai_verdict: u8`、`community_verdict: u8`、`reporter: address`、`timestamp: u64`
- **`voting_session` 模块**：`create_session(record: &mut ModerationRecord, duration_epochs: u64)` 创建投票会话
- **`vote_cast` 模块**：`cast_vote(session: &mut VotingSession, voter: address, vote: u8, weight: u64)` 记录投票
- 关键数据结构：`VoteRecord { voter: address, vote: u8, weight: u64, timestamp: u64 }`

### 前端 / 后端
- **前端**：React + Next.js，内容审核队列、AI 审核详情、投票面板、审核统计仪表盘
- **后端**：Python Agent 服务，内容分析引擎、AI 审核管道、投票管理器
- 关键功能：内容提交、审核状态追踪、争议投票、审核历史、举报管理

### AI / Agent 部分
- 内容分类：多标签分类器检测垃圾信息、仇恨言论、恶意链接和违规内容
- 置信度评估：模型输出审核置信度，低置信度内容自动转社区投票
- 上下文理解：GPT-4o 理解内容语义和上下文，减少误判和漏判
- Agent 工作流：内容提交 → AI 分析 → 置信度评估 → 高置信直接裁决/低置信转投票 → 结果链上记录

## 技术架构

用户提交内容后，AI Agent 立即进行初审分析。多标签分类器检测违规类型，GPT-4o 进行语义理解审核。高置信度的违规内容直接标记为拒绝，高置信度的安全内容直接通过。低置信度或有争议的内容触发链上投票流程，创建 `VotingSession` 共享对象。社区持代币者在投票期内通过 PTB 投票。投票结束后自动统计并更新 `ModerationRecord` 对象状态。前端提供审核队列和投票面板。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 审核记录、投票会话和计票逻辑 |
| 前端 | ⭐⭐⭐ | 审核队列、投票面板和统计仪表盘 |
| AI/ML | ⭐⭐⭐⭐ | 内容分类、语义审核和置信度评估 |
| 集成复杂度 | ⭐⭐⭐ | AI 服务与链上投票的协调 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
