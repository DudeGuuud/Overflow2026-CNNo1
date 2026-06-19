# Competitor Analyzer / 竞争对手分析器

> 持续追踪竞品动态，分析报告存入 Walrus，自动对比变化并高亮重要更新 / Continuously tracks competitor dynamics with analysis reports in Walrus, auto-diffing and highlighting key changes

## 解决什么问题

Web3 项目需要持续关注竞争对手的产品更新、融资动态、社区活跃度等，但手动追踪耗时且容易遗漏。需要一个自动化 Agent 系统性地收集竞品信息、分析变化趋势、并以可追溯的方式存储分析结果，支持跨时间对比。

## 核心功能

- 竞品信息自动采集：监控竞品的 GitHub、Twitter、官网、博客等渠道
- 变化自动对比：将最新分析结果与历史数据对比，自动高亮新增和变化内容
- 竞品矩阵报告：定期生成多维度竞品对比矩阵（功能、用户量、TVL 等）
- 关键动态告警：竞品发布重大更新时即时通知

## 使用的 Walrus / Sui 能力

| 能力 | 用途 |
|------|------|
| Walrus Blob 存储 | 存储竞品分析报告、原始数据快照、对比差异报告 |
| MemWal 记忆层 | 记住竞品的历史基线数据，用于变化检测和趋势判断 |
| Sui 对象模型 | 链上记录竞品注册信息、分析任务配置、报告索引 |
| Walrus 读取 API | 获取历史分析数据进行对比 |

## 实现方案

### Agent / AI 部分
- 使用 CrewAI 构建分析团队：信息采集 Agent、分析 Agent、报告 Agent
- LLM 使用 GPT-4o 进行深度分析和对比推理
- 网页内容提取使用 FireCrawl API
- 变化检测使用文本相似度算法（余弦相似度）+ LLM 语义判断

### Walrus 集成
- 竞品快照以 `competitors/{name}/snapshot/{date}.json` 存储
- 对比报告以 `competitors/{name}/diff/{date}.md` 存储
- 矩阵报告以 `matrix/{date}.json` 存储
- Sui 链上 CompetitorRegistry 对象管理竞品列表和 Blob 索引

### 前端 / 后端
- 前端：Next.js + Ant Design，展示竞品矩阵和变化对比
- 后端：Python FastAPI + APScheduler 定时调度
- 关键页面：竞品总览页、单竞品详情页、变化对比页、矩阵报告页

## 技术架构

用户注册竞品列表 → 定时调度器触发采集 Agent → 多渠道并行抓取竞品信息 → 分析 Agent 与 MemWal 中的历史基线对比 → 生成变化报告和矩阵 → 所有结果写入 Walrus Blob → Sui 链上更新索引 → 前端查询渲染。重大变化触发 Webhook 告警。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Walrus 集成 | ⭐⭐ | 快照 + 对比报告的分层存储 |
| AI/Agent | ⭐⭐⭐ | 多源采集和语义变化检测 |
| 前端 | ⭐⭐⭐ | 矩阵展示和变化高亮 |
| 集成复杂度 | ⭐⭐⭐ | 多渠道集成 + 定时调度 + 变化检测 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
