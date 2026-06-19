# DAO Governance AI Assistant / DAO 治理 AI 助手

> 分析提案内容，AI 生成摘要、影响分析和投票建议

## 解决什么问题

DAO 治理面临参与率低和信息不对称的困境。大量提案涉及复杂的技术和经济参数，普通持有者难以理解其影响，导致投票集中在少数大户手中。本项目通过 AI 深度分析 DAO 提案内容，生成通俗易懂的摘要、多维影响评估和个性化投票建议，降低治理参与门槛，提升 DAO 决策质量。

## 核心功能

- AI 自动解析提案文本和关联的 Move 合约变更，生成中英文摘要和关键变更点列表
- 多维影响分析：评估提案对代币经济、安全性、用户体验和协议竞争力的影响
- 个性化投票建议：根据用户持仓和风险偏好，AI 生成投票建议并解释理由

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `ProposalAnalysis` 对象存储 AI 分析结果，关联原始提案 |
| 事件（Event） | 订阅 DAO 提案创建事件，触发 AI 分析流程 |
| 链上数据 | 读取提案状态、投票记录和代币分布，辅助影响分析 |
| PTB | 将「读取分析 → 用户确认 → 提交投票」打包为原子化交易 |

## 实现方案

### 智能合约 (Move)
- **`proposal_analysis` 模块**：定义 `ProposalAnalysis` 对象，字段包括 `proposal_id: ID`、`summary_hash: vector<u8>`、`impact_score: u64`、`risk_level: u8`、`vote_recommendation: u8`、`analysis_epoch: u64`、`analyzer: address`
- **`impact_dimension` 模块**：`add_dimension(analysis: &mut ProposalAnalysis, dimension: u8, score: u64, evidence: vector<u8>)` 添加影响维度评分
- **`vote_record` 模块**：`record_vote(analysis: &ProposalAnalysis, voter: address, vote: u8, reasoning_hash: vector<u8>)` 记录投票理由
- 关键数据结构：`ImpactDimension { dimension: u8, score: u64, description: String, affected_contracts: vector<ID> }`

### 前端 / 后端
- **前端**：React + Next.js，提案分析看板、影响雷达图、投票建议卡片、历史投票追踪
- **后端**：Python Agent 服务，提案解析器、影响评估引擎、推荐生成器
- 关键功能：提案列表、AI 摘要、影响分析、投票建议、社区共识面板

### AI / Agent 部分
- 提案解析：GPT-4o 分析提案文本和 Move 代码变更，提取关键变更点和影响范围
- 影响评估：多维度评分模型评估提案对安全性、经济模型、用户体验、竞争力的综合影响
- 投票建议：基于用户持仓画像和风险偏好的个性化推荐引擎
- Agent 工作流：监听提案事件 → 解析提案内容 → 代码变更分析 → 影响评估 → 生成摘要 → 链上存证

## 技术架构

Agent 服务通过 Sui WebSocket 订阅 DAO 提案创建事件。收到新提案后，解析器提取提案文本和关联的 Move 合约代码变更。GPT-4o 分析提案内容，生成通俗易懂的摘要和关键变更列表。影响评估引擎从安全性、经济模型、用户体验和协议竞争力四个维度评分。推荐生成器结合用户持仓和偏好生成个性化投票建议。分析结果写入 `ProposalAnalysis` Move 对象上链存证。前端展示交互式分析看板。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 分析结果存储和投票记录 |
| 前端 | ⭐⭐⭐ | 影响雷达图和分析看板 |
| AI/ML | ⭐⭐⭐⭐ | 提案理解、影响评估和推荐生成 |
| 集成复杂度 | ⭐⭐⭐ | DAO 提案解析和多维评估逻辑 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
