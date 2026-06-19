# On-chain Sentiment Analyst / 链上情感分析 Agent

> 抓取社交数据和链上交易，AI 生成市场情感指数，关联 Sui 生态代币价格

## 解决什么问题

加密市场情绪对价格有显著影响，但缺乏将链上数据与社交媒体情绪整合分析的工具。现有情感分析产品大多针对 BTC/ETH，对 Sui 生态代币覆盖不足。本项目同时抓取社交媒体讨论和链上交易数据，AI 生成针对 Sui 生态的综合情感指数，并将情绪指标与代币价格和交易量关联分析，为投资者提供市场情绪维度的决策参考。

## 核心功能

- 实时抓取 Twitter/X、Discord、Telegram 中关于 Sui 生态的讨论，NLP 分析情感倾向
- 同步聚合链上交易数据（交易量、大户活动、新地址流入），构建链上情绪指标
- 综合情感指数（0-100）每日更新，历史趋势可回溯
- 情绪指标与代币价格关联分析和预警（极度恐惧/贪婪信号）

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `SentimentIndex` 对象存储情感评分、关键指标和元数据 |
| 事件（Event） | 发出 `SentimentAlertEvent` 在情绪极端时通知 |
| 链上数据 | 直接读取交易量、地址活动等链上指标作为分析输入 |
| 动态字段 | 为不同 Sui 生态代币附加独立的情感评分和趋势 |

## 实现方案

### 智能合约 (Move)
- **`sentiment_index` 模块**：定义 `SentimentIndex` 共享对象，字段包括 `overall_score: u64`、`token_scores: Bag`、`last_update: u64`、`trend: u8`（rising/falling/stable）
- **`token_sentiment` 模块**：`update_token_score(index: &mut SentimentIndex, token_type: String, score: u64, volume_signal: u64)` 更新代币级情感评分
- **`alert_engine` 模块**：`check_extremes(index: &SentimentIndex)` 检测极端情绪并发出告警
- 关键数据结构：`TokenSentiment { token: String, score: u64, social_volume: u64, onchain_volume: u256, whale_activity: u64, timestamp: u64 }`

### 前端 / 后端
- **前端**：React + Recharts，情感指数仪表盘、代币热度排行、情绪 vs 价格叠加图
- **后端**：Python Agent 服务，社交媒体爬虫、NLP 分析、链上数据聚合、AI 推理
- 关键功能：实时情感仪表盘、历史回测、代币对比

### AI / Agent 部分
- NLP 情感分析：FinBERT 模型对社交媒体文本做金融情感分类（positive/neutral/negative）
- 链上情绪建模：统计链上指标（交易量变化、大户行为、新地址流入）计算链上情绪分数
- 综合评分：加权融合社交情绪和链上情绪，输出综合情感指数
- Agent 工作流：抓取社交数据 → NLP 分析 → 聚合链上指标 → 综合评分 → 更新链上对象 → 前端推送

## 技术架构

Agent 服务分为两个并行管道：社交管道通过 Twitter/Discord API 抓取 Sui 相关讨论，FinBERT 模型分析情感倾向和讨论热度；链上管道通过 Sui RPC 聚合交易量、大户活动和地址增长数据，计算链上情绪指标。两个管道的输出加权融合为综合情感指数，通过 PTB 更新 SentimentIndex 共享对象。前端订阅链上事件实时展示情感仪表盘，支持按代币筛选和历史趋势对比。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 情感指数存储和更新逻辑较简单 |
| 前端 | ⭐⭐⭐ | 情感仪表盘和价格关联可视化 |
| AI/ML | ⭐⭐⭐⭐ | FinBERT 微调和多源情感融合 |
| 集成复杂度 | ⭐⭐⭐⭐ | 社交媒体数据采集和链上数据聚合 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
