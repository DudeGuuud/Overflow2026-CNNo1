# Personal Knowledge Manager / 个人知识管理智能体

> 用户对话和笔记自动整理存储在 Walrus，形成持久可检索的个人知识库 / Auto-organizes conversations and notes in Walrus, forming a persistent searchable personal knowledge base

## 解决什么问题

人们与 AI 的对话中蕴含大量有价值的知识和灵感，但这些内容通常随会话结束而消失。用户需要一个能自动整理、分类、索引所有 AI 对话和笔记的系统，形成个人专属知识库，支持跨会话检索和引用，且数据完全由用户掌控。

## 核心功能

- 对话自动归档：与 AI 的每次对话自动提取关键知识点并存储
- 智能标签分类：Agent 自动为知识点添加主题标签和关联关系
- 语义检索：通过自然语言查询检索历史知识库中的相关内容
- 知识图谱：自动构建知识点之间的关联图谱，可视化知识结构

## 使用的 Walrus / Sui 能力

| 能力 | 用途 |
|------|------|
| Walrus Blob 存储 | 存储对话记录（JSON）、知识卡片（Markdown）、标签索引 |
| MemWal 记忆层 | 管理用户的长期知识偏好和检索习惯 |
| Sui 对象模型 | 链上记录知识库所有权、访问权限、标签索引 |
| Seal 隐私层 | 加密存储敏感笔记，仅用户可解密查看 |

## 实现方案

### Agent / AI 部分
- 使用 LangChain + LlamaIndex 构建知识管理 Agent
- 知识提取使用 GPT-4o，从对话中抽取结构化知识点
- 标签生成使用零样本分类 + 用户反馈微调
- 语义检索使用 text-embedding-3-large 生成向量索引

### Walrus 集成
- 对话记录以 `conversations/{date}/{session_id}.json` 存储
- 知识卡片以 `cards/{card_id}.md` 存储，包含标题、内容、标签、来源
- 标签索引以 `index/tags.json` 存储，维护标签到 Blob ID 的映射
- 敏感笔记使用 Seal 加密后存储，密钥由用户钱包签名管理

### 前端 / 后端
- 前端：Next.js + React Flow，展示知识卡片和关联图谱
- 后端：Python FastAPI + ChromaDB 向量检索
- 关键页面：知识卡片列表页、知识图谱可视化页、语义搜索页、对话回溯页

## 技术架构

用户与 AI 对话 → Agent 实时提取关键知识点 → 生成知识卡片并打标签 → 加密敏感内容（Seal）→ 写入 Walrus Blob → 标签和向量索引更新到 Sui 链上对象 → 用户检索时语义匹配 → 从 Walrus 获取对应卡片渲染。MemWal 提供个性化推荐上下文。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Walrus 集成 | ⭐⭐ | 多类型文件存储 + Seal 加密 |
| AI/Agent | ⭐⭐⭐ | 知识提取、标签分类、语义检索 |
| 前端 | ⭐⭐⭐ | 知识图谱可视化和交互式搜索 |
| 集成复杂度 | ⭐⭐⭐ | 向量索引 + 链上索引 + 加密解密 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
