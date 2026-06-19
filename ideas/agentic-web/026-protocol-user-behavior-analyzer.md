# Protocol User Behavior Analyzer / 协议用户行为分析

> AI 分析协议用户画像、留存率、活跃度，帮助项目方优化产品

## 解决什么问题

Sui 生态的 DeFi 协议项目方缺乏深入的用户行为分析工具，难以了解用户画像、留存趋势和活跃模式。传统链上分析工具提供的数据维度有限，无法揭示用户行为背后的动机和模式。本项目利用 AI 分析协议用户的链上行为数据，生成用户画像、留存曲线和活跃度分层，帮助项目方数据驱动地优化产品和增长策略，分析结果链上存证确保数据透明。

## 核心功能

- 自动分析协议用户行为：交易频率、持仓变化、协议交互路径、活跃时段
- 用户分层：AI 将用户分为新用户、活跃用户、沉睡用户、大户等群体，生成画像标签
- 留存分析：按日/周/月计算用户留存率，识别留存漏斗中的流失节点
- 增长建议：AI 根据行为分析结果生成产品优化和增长策略建议

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `AnalysisReport` 对象存储用户分析结果和统计摘要 |
| 链上数据 | 读取协议事件和对象状态获取用户行为原始数据 |
| 动态字段 | 为不同用户群体附加画像标签和行为特征 |
| 事件（Event） | 发出 `ReportUpdatedEvent` 在新报告生成时通知协议方 |

## 实现方案

### 智能合约 (Move)
- **`analysis_report` 模块**：定义 `AnalysisReport` 对象，字段包括 `protocol_id: ID`、`total_users: u64`、`active_users_24h: u64`、`active_users_7d: u64`、`retention_d1: u64`、`retention_d7: u64`、`retention_d30: u64`、`updated_at: u64`
- **`user_segment` 模块**：`record_segment(report: &mut AnalysisReport, segment_type: u8, user_count: u64, avg_balance: u256)` 记录用户分层数据
- **`growth_metric` 模块**：`update_metrics(report: &mut AnalysisReport, new_users: u64, churned_users: u64, reactivated: u64)` 更新增长指标
- 关键数据结构：`UserSegment { segment_type: u8, user_count: u64, avg_tx_per_day: u64, avg_balance: u256, avg_tenure_epochs: u64 }`

### 前端 / 后端
- **前端**：React + ECharts，用户画像看板、留存曲线图、活跃度热力图、漏斗分析
- **后端**：Python Agent 服务，用户行为数据管道、聚类分析、留存计算、报告生成
- 关键功能：协议选择、时间范围筛选、用户分群对比、增长建议面板

### AI / Agent 部分
- 用户聚类：K-Means + RFM 模型（Recency, Frequency, Monetary）将用户分为行为群体
- 留存预测：生存分析模型（Cox Proportional Hazards）预测用户流失概率
- 画像生成：GPT-4o 基于统计数据生成自然语言用户画像描述和增长建议
- Agent 工作流：每日同步协议事件 → 更新用户行为表 → 运行聚类和留存模型 → 生成分析报告 → 链上更新

## 技术架构

Agent 服务每日通过 Sui RPC 和索引服务同步目标协议的所有用户交互事件（存款、借款、交易等）。行为数据管道将原始事件转化为用户行为特征矩阵，K-Means+RFM 聚类将用户分为行为群体。留存计算引擎按日/周/月统计各群体留存率。Cox 模型预测用户流失风险。分析结果通过 PTB 更新 AnalysisReport 链上对象，GPT-4o 生成可读的洞察摘要和增长建议。前端展示完整的分析看板。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 分析报告存储和指标更新 |
| 前端 | ⭐⭐⭐ | 分析看板和多种图表可视化 |
| AI/ML | ⭐⭐⭐ | 用户聚类、留存分析和流失预测模型 |
| 集成复杂度 | ⭐⭐⭐ | 协议事件数据采集和行为特征工程 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
