# NFT Rarity & Valuation Agent / NFT 稀有度与估值 Agent

> 分析链上 NFT 属性和交易历史，AI 生成估值报告和稀有度排名

## 解决什么问题

Sui 生态 NFT 市场缺乏专业的稀有度分析和估值工具，买家难以判断 NFT 的真实价值。稀有度排名通常由中心化平台提供，缺乏透明度和可验证性。交易历史和属性数据分散在链上各处，手动分析效率极低。本项目聚合链上 NFT 属性和交易数据，AI 生成稀有度排名和估值报告，结果链上存证确保公正透明。

## 核心功能

- 自动解析 NFT 集合的 metadata 属性，计算基于统计的稀有度排名（trait rarity + statistical rarity）
- AI 估值模型综合属性稀有度、交易历史、市场趋势和同类成交价生成估值报告
- 链上稀有度注册表：排名结果写入 Move 对象，任何人可验证
- 集合级别的市场分析：地板价走势、成交量分布、持有者集中度

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `RarityRegistry` 对象存储集合的稀有度排名和估值数据 |
| 动态字段 | 为每个 NFT ID 附加稀有度分数、排名和估值 |
| 链上数据 | 读取 NFT 对象的 metadata 属性和交易事件 |
| 事件（Event） | 发出 `ValuationUpdatedEvent` 在估值变化时通知 |

## 实现方案

### 智能合约 (Move)
- **`rarity_registry` 模块**：定义 `RarityRegistry` 对象，字段包括 `collection_id: ID`、`total_supply: u64`、`last_update: u64`
- **`nft_ranking` 模块**：`update_ranking(registry: &mut RarityRegistry, nft_id: ID, rarity_score: u64, rank: u64, estimated_value: u64)` 更新单个 NFT 的排名数据
- **`collection_analytics` 模块**：`update_collection_stats(registry: &mut RarityRegistry, floor_price: u64, volume_24h: u256, holder_count: u64)` 更新集合级统计
- 关键数据结构：`NFTRanking { nft_id: ID, rarity_score: u64, rank: u64, estimated_value: u64, trait_scores: vector<u64> }`

### 前端 / 后端
- **前端**：React + TailwindCSS，NFT 详情页（稀有度标签、估值卡片）、集合排行表、市场分析图表
- **后端**：Python Agent 服务，metadata 解析、稀有度计算引擎、估值模型
- 关键功能：集合搜索、稀有度排行、估值报告导出

### AI / Agent 部分
- 稀有度算法：Jaccard 稀有度 + 统计稀有度 + trait 分数加权融合
- 估值模型：梯度提升树（XGBoost）基于属性稀有度、历史成交价、市场趋势和持有时间预测价值
- 趋势分析：GPT-4o 生成自然语言估值分析报告，解释定价依据
- Agent 工作流：解析集合 metadata → 计算稀有度 → 运行估值模型 → 更新链上排名 → 生成报告

## 技术架构

Agent 服务通过 Sui RPC 获取目标 NFT 集合所有 token 的 metadata 属性和交易历史。稀有度引擎统计各属性的出现频率，计算多维稀有度分数。XGBoost 估值模型综合稀有度、近期成交价和市场趋势预测 NFT 价值。排名和估值结果通过 PTB 批量写入 RarityRegistry 链上对象。前端展示排行表和 NFT 详情页，GPT-4o 生成可读的估值分析报告。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 稀有度注册表和排名存储 |
| 前端 | ⭐⭐⭐ | NFT 展示和排行表格 UI |
| AI/ML | ⭐⭐⭐ | 稀有度算法和估值模型训练 |
| 集成复杂度 | ⭐⭐⭐ | NFT metadata 解析和市场数据聚合 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
