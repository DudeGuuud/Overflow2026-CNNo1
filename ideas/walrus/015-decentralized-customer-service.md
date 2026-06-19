# Decentralized Customer Service / 去中心化客服系统

> 多个客服 Agent 协作处理用户问题，对话历史存入 Walrus，避免重复回答 / Multiple customer service Agents collaboratively handle user questions with conversation history in Walrus to avoid duplicate answers

## 解决什么问题

Web3 项目的客服面临用户量大、问题重复、24/7 需求等挑战。多个 AI Agent 可以协作处理不同类型的问题，但需要共享对话历史来避免重复回答和保持服务一致性。Walrus 提供持久化的对话存储，确保所有 Agent 都能访问完整的服务历史。

## 核心功能

- 多 Agent 分工协作：FAQ Agent 处理常见问题、技术 Agent 处理复杂问题、升级 Agent 传递人工
- 共享知识库：所有 Agent 共享 Walrus 上的 FAQ 知识库和历史对话记录
- 对话连续性：用户可跨会话继续之前未解决的问题
- 服务质量追踪：记录每个 Agent 的响应质量和用户满意度

## 使用的 Walrus / Sui 能力

| 能力 | 用途 |
|------|------|
| Walrus Blob 存储 | 存储对话记录（JSON）、FAQ 知识库、服务报告 |
| MemWal 记忆层 | 各 Agent 共享用户上下文和已解决问题的记忆 |
| Sui 对象模型 | 链上记录工单状态、Agent 分配、服务质量评分 |
| Walrus 读取 API | Agent 快速检索历史对话和知识库 |

## 实现方案

### Agent / AI 部分
- 使用 LangGraph 构建客服工作流路由
- FAQ Agent：基于 RAG 检索知识库，GPT-4o-mini 快速响应
- 技术 Agent：GPT-4o 处理链上操作指导等复杂问题
- 路由 Agent：分类用户意图并分配给合适的 Agent
- 知识库使用 LlamaIndex + ChromaDB 构建 RAG 管道

### Walrus 集成
- 对话记录以 `conversations/{ticket_id}/messages.json` 存储
- FAQ 知识库以 `knowledge/faq.json` 存储，定期更新
- 服务报告以 `reports/{date}/daily.md` 存储
- Sui 链上 Ticket 对象管理工单生命周期和状态转换

### 前端 / 后端
- 前端：React 聊天界面，支持 WebSocket 实时对话
- 后端：Python FastAPI + Socket.IO，管理 Agent 调度
- 关键页面：用户聊天窗口、Agent 管理面板、知识库编辑页、服务统计页

## 技术架构

用户发起对话 → 路由 Agent 分类意图 → 分配给专业 Agent → Agent 从 Walrus 读取知识库和历史对话 → 生成回答 → 对话记录写入 Walrus → 更新 Sui 链上工单状态 → 如需升级则通知人工 → 定时生成服务报告写入 Walrus。MemWal 维护跨会话的用户上下文。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Walrus 集成 | ⭐⭐ | 对话记录和知识库的频繁读写 |
| AI/Agent | ⭐⭐⭐ | 意图分类准确度和多 Agent 路由 |
| 前端 | ⭐⭐⭐ | 实时聊天界面和 Agent 管理面板 |
| 集成复杂度 | ⭐⭐⭐ | 多 Agent 调度 + 实时通信 + 知识库管理 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
