# Community Contribution Tracker / 社区贡献追踪器

> AI 追踪和评估成员在 Sui 生态的贡献（代码、内容、社区活动），生成贡献度排名

## 解决什么问题

Sui 生态的社区贡献缺乏统一的追踪和评估标准，贡献者的付出难以量化和获得认可。项目方无法有效识别活跃贡献者并进行激励分配。传统的贡献追踪依赖手动记录和主观评价，覆盖面窄且容易遗漏。本项目通过 AI 自动追踪社区成员在代码提交、内容创作、社区活动等多维度的贡献，生成客观的贡献度排名和评估报告，链上存证确保公平透明。

## 核心功能

- 多源贡献数据采集：GitHub 代码提交、文档编写、社区活动参与、Discord 活跃度、测试网反馈
- AI 评估贡献质量和影响力：不仅统计数量，还评估代码质量、内容影响力、社区贡献深度
- 生成贡献度排名和徽章，支持项目方自定义权重和激励规则

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `ContributorProfile` 对象归属贡献者，存储总积分和各维度得分 |
| 动态字段 | 为不同贡献类型（代码、文档、社区、测试）附加详细记录和积分明细 |
| NFT | 贡献徽章以 NFT 形式发放，可转移或展示，体现贡献者身份 |
| 事件（Event） | 发出 `ContributionRecordedEvent` 通知贡献已被记录和评估 |

## 实现方案

### 智能合约 (Move)
- **`contributor_profile` 模块**：定义 `ContributorProfile` 对象，字段包括 `contributor: address`、`total_score: u64`、`code_score: u64`、`content_score: u64`、`community_score: u64`、`test_score: u64`、`contribution_count: u64`、`joined_at: u64`
- **`contribution_record` 模块**：`record_contribution(profile: &mut ContributorProfile, category: u8, score: u64, evidence_hash: vector<u8>)` 记录单次贡献
- **`badge` 模块**：`mint_badge(profile: &ContributorProfile, badge_type: u8): Badge` 铸造贡献徽章 NFT
- 关键数据结构：`Contribution { category: u8, description: String, score: u64, evidence_url: String, timestamp: u64 }`

### 前端 / 后端
- **前端**：React + TailwindCSS，贡献者排行榜、个人贡献面板、徽章展示墙、项目仪表盘
- **后端**：Python Agent 服务，多源数据采集器、贡献评估模型、积分计算引擎
- 关键功能：排行榜、贡献详情、徽章管理、项目贡献看板、导出报告

### AI / Agent 部分
- 代码评估：GPT-4o 分析 PR 的代码质量、复杂度和影响范围，量化代码贡献
- 内容评估：AI 评估文档/教程的完整性、准确性和社区影响力
- 综合评分：多维度加权评分模型，项目方可自定义各维度权重
- Agent 工作流：多源数据采集 → 贡献识别 → 质量评估 → 积分计算 → 排名更新 → 链上记录

## 技术架构

Agent 服务定期从 GitHub（PR/Issue/Review）、文档平台、Discord 和测试网采集社区活动数据。AI 评估引擎分析每项贡献的质量和影响力，GPT-4o 理解代码 PR 的技术深度和文档的完整性。评分引擎根据项目方配置的权重计算综合积分。积分和贡献记录更新 `ContributorProfile` Move 对象，里程碑时自动铸造贡献徽章 NFT。前端展示排行榜和个人贡献档案。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 贡献档案、记录和徽章 NFT 管理 |
| 前端 | ⭐⭐⭐ | 排行榜、贡献面板和徽章展示 |
| AI/ML | ⭐⭐⭐ | 贡献质量评估和综合评分模型 |
| 集成复杂度 | ⭐⭐⭐⭐ | 多平台数据采集和贡献去重 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
