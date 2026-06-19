# Multi-Agent Debate System / 多智能体辩论系统

> 多个 AI Agent 对同一问题从不同角度分析，辩论过程存储在 Walrus，生成综合结论 / Multiple AI Agents analyze the same question from different perspectives, storing debate process in Walrus with synthesized conclusions

## 解决什么问题

单一 AI 的分析容易受模型偏见影响，缺乏多角度审视。通过让多个 Agent 扮演不同角色（乐观者、悲观者、分析师、风控官）进行结构化辩论，可以得到更全面、更平衡的结论。辩论过程的完整记录存储在 Walrus，保证透明可审计。

## 核心功能

- 多角色 Agent 配置：支持自定义 Agent 角色、立场和分析框架
- 结构化辩论流程：依次进行开篇陈述、交叉质询、总结陈词
- 实时辩论可视化：前端实时展示各 Agent 的论点和反驳
- 综合结论生成：辩论结束后由裁判 Agent 生成综合结论和各方观点摘要

## 使用的 Walrus / Sui 能力

| 能力 | 用途 |
|------|------|
| Walrus Blob 存储 | 存储辩论完整记录（JSON）、各方论点、裁判结论 |
| MemWal 记忆层 | 各 Agent 记住历史辩论中使用的论点和被反驳的策略 |
| Sui 对象模型 | 链上记录辩论主题、参与者、结论哈希，确保不可篡改 |
| Walrus 读取 API | 回溯历史辩论记录和结论 |

## 实现方案

### Agent / AI 部分
- 使用 AutoGen GroupChat 管理多 Agent 对话
- 每个 Agent 使用 GPT-4o，通过 system prompt 注入不同角色和立场
- 裁判 Agent 使用 Claude 3.5 Sonnet 进行综合评判
- 辩论流程通过 LangGraph StateGraph 定义：陈述 → 质疑 → 反驳 → 总结

### Walrus 集成
- 辩论记录以 `debates/{debate_id}/full_record.json` 存储
- 各方论点摘要以 `debates/{debate_id}/positions/{agent_id}.md` 存储
- 裁判结论以 `debates/{debate_id}/verdict.md` 存储
- Sui 链上 DebateRecord 对象记录主题、参与者 Blob IDs、结论哈希

### 前端 / 后端
- 前端：React + Framer Motion，动画展示辩论过程
- 后端：Python FastAPI + WebSocket 实时推送辩论消息
- 关键页面：辩论发起页、实时辩论观赏页、历史辩论回顾页、结论对比页

## 技术架构

用户发起辩论主题 → 创建多个角色 Agent → 辩论按流程推进 → 各 Agent 轮流发言 → 每轮发言内容实时推送到前端 → 辩论结束后裁判 Agent 生成结论 → 完整记录和结论写入 Walrus Blob → Sui 链上创建 DebateRecord → 用户可随时回溯历史辩论。MemWal 让 Agent 记住历史辩论策略。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Walrus 集成 | ⭐⭐ | 辩论记录的结构化存储 |
| AI/Agent | ⭐⭐⭐⭐ | 多 Agent 协调、角色一致性、辩论质量 |
| 前端 | ⭐⭐⭐ | 实时辩论动画和交互展示 |
| 集成复杂度 | ⭐⭐⭐⭐ | 多 Agent 调度 + 实时通信 + 记录同步 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
