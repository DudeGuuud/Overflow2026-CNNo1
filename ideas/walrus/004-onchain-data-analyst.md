# On-chain Data Analysis Agent / 链上数据分析智能体

> 长期收集 Sui 链上数据，生成趋势分析报告存储在 Walrus，支持时间线回溯 / Long-term Sui on-chain data collection with trend analysis reports stored in Walrus and timeline playback

## 解决什么问题

链上数据分析通常是碎片化的，缺乏长期趋势追踪和历史对比能力。开发者、投资者和研究者需要一个持续运行的分析 Agent，自动收集链上数据、发现趋势变化、并将分析结果永久存储以便回溯验证。

## 核心功能

- 自动链上数据采集：定期采集 Sui 网络 TVL、交易量、活跃地址、Gas 费等核心指标
- 趋势分析报告：AI 自动识别异常波动、趋势变化，生成结构化分析报告
- 时间线回溯：用户可选择任意时间范围查看历史分析，所有报告存储在 Walrus
- 自定义看板：用户可自定义关注的指标和告警阈值

## 使用的 Walrus / Sui 能力

| 能力 | 用途 |
|------|------|
| Walrus Blob 存储 | 存储数据快照（Parquet/JSON）、分析报告（Markdown）、趋势图表数据 |
| MemWal 记忆层 | 记录历史趋势判断和异常事件，用于新数据分析时的上下文参考 |
| Sui 对象模型 | 链上记录分析任务配置、数据快照索引、报告元数据 |
| Sui GraphQL API | 高效查询链上历史交易和事件数据 |

## 实现方案

### Agent / AI 部分
- 使用 LangGraph 构建数据采集 → 分析 → 报告生成的工作流
- 数据采集通过 Sui TypeScript SDK + GraphQL 获取链上数据
- LLM 使用 GPT-4o 进行趋势分析和异常检测
- 时序异常检测辅助使用 Prophet 库

### Walrus 集成
- 数据快照以 `snapshots/{metric}/{date}.json` 存储
- 分析报告以 `reports/weekly/{week}.md` 和 `reports/daily/{date}.md` 存储
- 趋势图表的配置和数据以 `charts/{chart_id}.json` 存储
- Sui 链上 AnalyticsIndex 对象维护所有 Blob 的日期索引

### 前端 / 后端
- 前端：Next.js + Recharts，交互式图表和时间范围选择器
- 后端：Node.js + Express，提供数据查询 API
- 关键页面：总览仪表盘、趋势详情页、历史回溯页、自定义看板页

## 技术架构

定时调度器（每小时）→ Sui GraphQL 采集链上指标 → 数据暂存 Redis → Agent 分析趋势和异常 → 生成分析报告 → 报告和快照写入 Walrus Blob → Sui 链上更新索引对象 → 前端通过索引查询历史 Blob → 从 Walrus 获取数据渲染图表和报告。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Walrus 集成 | ⭐⭐ | 多类型 Blob（数据+报告）统一管理 |
| AI/Agent | ⭐⭐⭐ | 趋势分析和异常检测需要统计模型 |
| 前端 | ⭐⭐⭐ | 交互式图表和时间线展示 |
| 集成复杂度 | ⭐⭐⭐ | 数据采集 + 分析 + 存储的完整流水线 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
