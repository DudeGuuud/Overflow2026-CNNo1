# Multi-Agent Prediction Market / 多智能体预测市场

> 多个分析 Agent 各自预测，聚合结果存入 Walrus，追踪预测准确率 / Multiple analysis Agents make predictions independently, aggregating results in Walrus and tracking accuracy

## 解决什么问题

单一 AI 预测容易受模型偏差影响，准确率不稳定。通过让多个独立 Agent 各自分析并给出预测，再通过聚合算法（如加权平均、置信度投票）得到综合预测，可显著提高可靠性。Walrus 持久化存储每个 Agent 的预测记录，长期追踪准确率。

## 核心功能

- 多 Agent 独立预测：每个 Agent 使用不同策略和分析方法独立给出预测
- 预测聚合引擎：将多个 Agent 预测通过加权投票、贝叶斯聚合等算法综合
- 准确率排行榜：追踪每个 Agent 的历史预测准确率，动态调整聚合权重
- 预测市场创建：支持自定义预测事件（价格、TVL、事件发生概率等）

## 使用的 Walrus / Sui 能力

| 能力 | 用途 |
|------|------|
| Walrus Blob 存储 | 存储各 Agent 预测记录、聚合结果、准确率统计 |
| MemWal 记忆层 | 各 Agent 记住历史预测的成功/失败经验 |
| Sui 对象模型 | 链上记录预测事件、各 Agent 投票、结算结果 |
| Walrus 读取 API | 回溯历史预测进行准确率计算 |

## 实现方案

### Agent / AI 部分
- 使用 AutoGen 构建多个独立分析 Agent
- Agent 策略多样：技术分析 Agent（TA-Lib）、基本面 Agent（链上数据）、情绪 Agent（NLP）
- 每个 Agent 使用 GPT-4o 但注入不同 system prompt
- 聚合算法：贝叶斯模型平均（BMA）+ 历史准确率加权

### Walrus 集成
- 各 Agent 预测以 `predictions/{event_id}/{agent_id}.json` 存储
- 聚合结果以 `predictions/{event_id}/aggregated.json` 存储
- 准确率统计以 `stats/{agent_id}/accuracy.json` 存储
- Sui 链上 PredictionEvent 对象记录事件定义和结算状态

### 前端 / 后端
- 前端：React + Recharts，展示预测对比和准确率趋势
- 后端：Python FastAPI + Celery 定时调度
- 关键页面：预测事件列表页、Agent 预测详情页、准确率排行榜、聚合分析页

## 技术架构

创建预测事件 → 定时触发各分析 Agent → 各 Agent 独立分析并给出预测 → 预测写入 Walrus → 聚合引擎读取所有预测 → 计算加权综合预测 → 聚合结果写入 Walrus → Sui 链上记录 → 事件结算后计算准确率 → 更新 Agent 权重 → 准确率统计写入 Walrus。MemWal 帮助各 Agent 从历史中学习。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Walrus 集成 | ⭐⭐ | 预测记录和统计数据的读写 |
| AI/Agent | ⭐⭐⭐⭐ | 多策略预测和聚合算法 |
| 前端 | ⭐⭐⭐ | 预测对比图表和排行榜 |
| 集成复杂度 | ⭐⭐⭐⭐ | 多 Agent 调度 + 聚合计算 + 准确率追踪 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
