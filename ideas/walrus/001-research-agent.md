# Research Agent / 持续研究智能体

> AI Agent 持续追踪指定研究主题，将研究笔记、摘要、引用文献存储在 Walrus，跨会话积累可验证知识库 / Continuously tracks research topics, storing notes and citations in Walrus for cross-session knowledge accumulation

## 解决什么问题

传统 AI 对话缺乏长期记忆，每次研究都要从头开始。研究人员需要一个能持续追踪特定主题、自动积累知识、并在任意时间点可回溯的智能助手。Walrus 提供的持久化存储确保研究数据不会丢失，且所有结论可验证。

## 核心功能

- 主题订阅与持续追踪：用户指定研究主题，Agent 定期搜索最新论文、新闻、链上数据
- 研究笔记自动归档：将每日发现、摘要、关键引用以结构化 JSON 格式存储到 Walrus Blob
- 跨会话知识检索：Agent 在新会话中可读取历史研究笔记，基于之前积累的上下文继续深入
- 引用链可追溯：每条研究结论关联原始数据 Blob ID，任何人可通过链上哈希验证

## 使用的 Walrus / Sui 能力

| 能力 | 用途 |
|------|------|
| Walrus Blob 存储 | 存储研究笔记、论文摘要、引用数据的 JSON/Markdown 文件 |
| MemWal 记忆层 | 管理 Agent 的长期研究上下文，跨会话保持研究连贯性 |
| Sui 对象模型 | 链上记录研究主题元数据、Blob 引用关系、时间戳 |
| Walrus 读取 API | 按时间线检索历史研究 Blob，支持回溯任意日期 |

## 实现方案

### Agent / AI 部分
- 采用 LangGraph 构建 Agent 工作流，包含搜索节点、摘要节点、存储节点
- 使用 GPT-4o / Claude 3.5 进行论文理解和摘要生成
- 向量检索使用本地 FAISS 索引缓存，减少重复搜索
- 定时调度使用 Celery + Redis，每日自动执行研究任务

### Walrus 集成
- 每日研究笔记以 `research/{topic}/{date}.json` 命名存储为 Blob
- 元数据结构包含：topic、date、sources、summary、key_findings、blob_ids
- 读取时通过 Sui 链上对象查询 Blob ID 列表，再从 Walrus 批量获取内容
- 支持增量写入：追加新发现到已有 Blob 或创建新版本

### 前端 / 后端
- 前端：Next.js + TailwindCSS，展示研究时间线和笔记详情
- 后端：FastAPI 提供 Agent 调度 API 和 Walrus 数据查询接口
- 关键页面：主题管理页、研究时间线页、笔记详情页（含引用跳转）

## 技术架构

用户通过前端创建研究主题 → 后端调度 Agent 每日执行搜索 → Agent 将摘要和引用写入 Walrus Blob → Blob ID 和元数据记录到 Sui 链上对象 → 用户查看时间线时，前端通过 Sui 查询 Blob 列表 → 从 Walrus 读取并渲染研究笔记。MemWal 管理跨会话的研究上下文状态。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Walrus 集成 | ⭐⭐ | 标准 Blob 读写，结构化 JSON 存储 |
| AI/Agent | ⭐⭐⭐ | 需要持续调度和多步推理 |
| 前端 | ⭐⭐ | 时间线展示较直观 |
| 集成复杂度 | ⭐⭐⭐ | Agent 调度 + Walrus + 链上协调 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
