# Decentralized Prediction Aggregator / 去中心化预测聚合器

> AI 聚合多个数据源的预测结果，生成置信度评分，结果上链可审计

## 解决什么问题

链上预测市场和分析服务各自独立运行，预测结果分散且缺乏统一的置信度评估。用户无法比较不同来源的预测质量，也缺乏聚合多源预测的集成工具。单一的预测源容易受到偏见和操纵的影响。本项目通过 AI 聚合多个数据源（预言机、预测市场、链上分析模型）的预测结果，生成加权置信度评分，聚合结果和评分过程链上可审计，为 DeFi 协议和投资者提供更可靠的决策参考。

## 核心功能

- 多源预测采集：聚合链上预言机价格、预测市场赔率、链上分析模型的预测结果
- AI 加权聚合：根据各数据源的历史准确度和可靠性，加权生成综合预测和置信度评分
- 结果上链：聚合预测和置信度写入 Move 对象，其他合约和协议可直接引用

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `PredictionPool`、`AggregateResult` 对象管理预测来源和聚合结果 |
| 共享对象 | `PredictionPool` 共享对象允许多方提交和更新预测数据 |
| PTB | 将「采集多源预测 → AI 聚合 → 计算置信度 → 上链存储」编排为原子流程 |
| 事件（Event） | 发出 `PredictionUpdatedEvent`、`AggregateCompletedEvent` 通知新预测可用 |

## 实现方案

### 智能合约 (Move)
- **`prediction_pool` 模块**：定义 `PredictionPool` 对象，字段包括 `topic: String`、`resolution_epoch: u64`、`source_count: u64`、`latest_aggregate: vector<u8>`、`confidence_score: u64`、`total_submissions: u64`、`created_at: u64`
- **`source_submission` 模块**：`submit_prediction(pool: &mut PredictionPool, source_id: address, prediction: vector<u8>, confidence: u64, evidence: vector<u8>)` 提交预测
- **`aggregate_result` 模块**：`store_aggregate(pool: &mut PredictionPool, aggregate_data: vector<u8>, confidence: u64, source_weights: vector<u64>)` 存储聚合结果
- 关键数据结构：`SourcePrediction { source_id: address, prediction: vector<u8>, confidence: u64, weight: u64, timestamp: u64, historical_accuracy: u64 }`

### 前端 / 后端
- **前端**：React + Recharts，预测看板、多源对比图、置信度仪表盘、历史准确率
- **后端**：Python Agent 服务，多源数据采集器、聚合引擎、置信度模型、准确率追踪
- 关键功能：话题浏览、预测对比、聚合结果、来源评级、历史回测

### AI / Agent 部分
- 来源评估：AI 根据历史预测准确率动态调整各数据源的权重
- 预测聚合：加权平均、贝叶斯聚合和集成学习方法融合多源预测
- 置信度评分：根据来源一致性、历史准确度和数据新鲜度计算综合置信度
- Agent 工作流：多源数据采集 → 来源权重更新 → 预测聚合 → 置信度计算 → 结果上链 → 准确率追踪

## 技术架构

Agent 服务定期从多个数据源采集预测数据：链上预言机价格、Polymarket/类似预测市场赔率、链上分析模型输出。每个数据源通过 `submit_prediction` 函数将预测提交至 `PredictionPool` 共享对象。聚合引擎根据各来源的历史准确率动态计算权重，使用加权聚合方法生成综合预测。置信度模型综合来源一致性、数据新鲜度和历史表现计算置信评分。结果通过 PTB 写入 `AggregateResult` 对象上链。事件结算后追踪实际结果更新来源准确率。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 预测池管理、提交存储和聚合结果 |
| 前端 | ⭐⭐⭐ | 预测看板、多源对比和置信度可视化 |
| AI/ML | ⭐⭐⭐⭐ | 聚合算法、置信度建模和来源评估 |
| 集成复杂度 | ⭐⭐⭐⭐ | 多源数据采集、权重更新和结果验证 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
