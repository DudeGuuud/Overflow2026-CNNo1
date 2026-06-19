# Decentralized Content Creation / 去中心化内容创作

> 写作 Agent 写初稿，编辑 Agent 润色，审核 Agent 检查，所有版本存入 Walrus / Writing Agent drafts, Editing Agent polishes, Review Agent checks, with all versions stored in Walrus

## 解决什么问题

高质量内容的创作需要多个环节协作——写作、编辑、审核——但传统流程依赖人工协调，效率低且版本混乱。通过多 Agent 协作自动完成内容创作流水线，并将每个版本存储在 Walrus，确保内容演变过程完全可追溯、不可篡改。

## 核心功能

- 写作 Agent 自动生成初稿：根据主题和大纲生成结构化文章
- 编辑 Agent 多维度润色：语法、风格、逻辑流畅性、SEO 优化
- 审核 Agent 质量把关：事实核查、敏感词检测、合规审查
- 版本对比与追溯：展示从初稿到终稿的完整修订历史

## 使用的 Walrus / Sui 能力

| 能力 | 用途 |
|------|------|
| Walrus Blob 存储 | 存储每个版本的内容（Markdown）、修订记录、审核意见 |
| MemWal 记忆层 | 各 Agent 记住作者的写作风格偏好和常见问题 |
| Sui 对象模型 | 链上记录内容元数据、版本链、版权信息 |
| Walrus 读取 API | 读取历史版本进行对比和追溯 |

## 实现方案

### Agent / AI 部分
- 使用 CrewAI 构建三阶段内容流水线
- 写作 Agent：GPT-4o，擅长结构化写作和长文生成
- 编辑 Agent：Claude 3.5 Sonnet，擅长语言润色和逻辑优化
- 审核 Agent：GPT-4o + 规则引擎，检测事实错误和合规问题
- 工作流通过 LangGraph Pipeline 定义

### Walrus 集成
- 初稿以 `content/{id}/draft.md` 存储
- 编辑版本以 `content/{id}/edited/v{n}.md` 存储
- 审核意见以 `content/{id}/review/v{n}.json` 存储
- 终稿以 `content/{id}/final.md` 存储
- Sui 链上 ContentRecord 对象维护版本链

### 前端 / 后端
- 前端：Next.js + Slate.js 富文本编辑器，展示版本差异
- 后端：Python FastAPI，管理 Agent 流水线
- 关键页面：内容创建页、版本对比页（diff 视图）、审核意见页、内容列表页

## 技术架构

用户提交主题和大纲 → 写作 Agent 生成初稿 → 初稿写入 Walrus → 编辑 Agent 读取初稿进行润色 → 编辑版本写入 Walrus → 审核 Agent 读取编辑版本审查 → 审核意见写入 Walrus → 如需修改则循环 → 终稿写入 Walrus → Sui 链上更新版本链。MemWal 让各 Agent 学习作者风格。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Walrus 集成 | ⭐⭐ | 多版本链式存储 |
| AI/Agent | ⭐⭐⭐ | 写作质量控制和多 Agent 协调 |
| 前端 | ⭐⭐⭐ | 富文本展示和版本 diff 视图 |
| 集成复杂度 | ⭐⭐⭐ | 流水线调度 + 版本管理 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
