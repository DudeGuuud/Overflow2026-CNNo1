# News Tracker / 加密新闻追踪器

> AI 持续追踪加密新闻，生成每日摘要存入 Walrus，用户可回溯任意历史报告 / AI continuously tracks crypto news, generating daily summaries stored in Walrus with full historical access

## 解决什么问题

加密市场信息碎片化严重，投资者难以高效获取全貌。现有新闻聚合器缺乏 AI 深度分析能力，且历史报告难以检索和回溯。通过 Walrus 永久存储每日新闻摘要，用户可随时回溯任意日期的市场动态和 AI 分析。

## 核心功能

- 多源新闻聚合：自动抓取 CoinDesk、The Block、Twitter 等多个新闻源
- AI 摘要生成：将当日新闻提炼为结构化摘要，包含关键事件、市场影响、情绪评分
- 历史回溯检索：通过 Walrus 存储的日期索引，快速定位任意历史日期的新闻报告
- 主题关联分析：自动识别新闻之间的关联，构建事件脉络图

## 使用的 Walrus / Sui 能力

| 能力 | 用途 |
|------|------|
| Walrus Blob 存储 | 存储每日新闻摘要（Markdown）、原始新闻快照（JSON） |
| MemWal 记忆层 | 记录用户关注的主题和偏好，影响新闻筛选权重 |
| Sui 对象模型 | 链上索引每日摘要 Blob ID，支持按日期查询 |
| Walrus 读取 API | 按需获取历史新闻 Blob 内容 |

## 实现方案

### Agent / AI 部分
- 使用 LangChain 构建 News Agent，集成 NewsAPI、Twitter API、RSS 源
- LLM 使用 Claude 3.5 Sonnet 进行深度摘要和关联分析
- 情绪分析使用 HuggingFace FinBERT 模型
- 定时任务通过 GitHub Actions 或 Cloudflare Workers 每日触发

### Walrus 集成
- 每日摘要存储为 `news/daily/{YYYY-MM-DD}.md` Blob
- 原始新闻数据存储为 `news/raw/{YYYY-MM-DD}.json` Blob
- Sui 链上创建 DailySummary 对象，包含日期、Blob IDs、情绪评分
- MemWal 存储用户偏好和长期趋势判断

### 前端 / 后端
- 前端：Astro + React，静态生成 + 客户端交互
- 后端：Cloudflare Workers 处理新闻抓取和 AI 处理
- 关键页面：今日摘要页、历史日历页、主题追踪页

## 技术架构

定时触发器 → 新闻抓取模块并行获取多源数据 → AI Agent 过滤去重并生成摘要 → 摘要写入 Walrus Blob → Sui 链上记录日期索引 → 用户访问时前端查询链上日期索引 → 从 Walrus 获取对应 Blob 内容 → 渲染新闻报告。MemWal 在筛选阶段提供用户偏好上下文。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Walrus 集成 | ⭐⭐ | 按日期的标准化 Blob 存储模式 |
| AI/Agent | ⭐⭐⭐ | 多源聚合和摘要质量需要调优 |
| 前端 | ⭐⭐ | 日历式历史浏览较直观 |
| 集成复杂度 | ⭐⭐⭐ | 多源数据清洗 + 定时调度 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
