# Walrus

**Walrus 可验证数据**

## Problem Statement / 核心命题

AI agents today are powerful, but still fundamentally stateless and fragmented. They complete tasks in isolation, lose context across sessions, and struggle to share knowledge across tools, teams, or workflows. Memory is often tied to a single app, model, or device — making agent systems brittle, hard to scale, and difficult to trust.

AI 智能体虽然强大，但本质上仍然是无状态和碎片化的。它们在隔离中完成任务，跨会话丢失上下文，难以在工具、团队或工作流之间共享知识。记忆通常绑定在单个应用、模型或设备上——导致智能体系统脆弱、难以扩展且难以信任。

As agents evolve from simple assistants to autonomous, long-running systems, they need a more durable foundation:
- 存储和检索跨会话的记忆
- 在智能体和工作流之间共享上下文
- 访问可移植、持久、不被锁定在单一平台的数据

**本赛道挑战你重新思考智能体系统的构建方式，将 Walrus 作为 AI 的可验证数据平台。**

## Prize / 奖金

| Place | Prize |
|-------|-------|
| 1st | $35,000 |
| 2nd | $15,000 |
| 3rd | $7,500 |
| 4th | $5,000 |

## 参考方向 / Suggested Topics

构建功能性 AI Agent 或智能体工作流（单智能体或多智能体），覆盖任意领域——金融、生产力、游戏等，需要展示：

1. **使用 Walrus 的持久数据和文件访问**（直接使用或通过文件管理接口）
2. **集成和工具**，使开发者更容易在智能体系统中采用 Walrus 或 MemWal（Walrus Memory）

### 智能体应用方向

- **长期运行的工作流**——Agent 跨时间追踪状态（如研究 Agent、交易 Agent、监控系统）
- **多智能体协作**——如协商、任务委派、跨 Agent 的分步执行
- **产物驱动的工作流**——Agent 生成、存储和复用文件（数据集、日志、报告、中间输出）

### 集成与工具方向

- 为现有 Agent 框架添加持久记忆（插件或适配器，直接使用 Walrus 或 MemWal 作为记忆层）
- 创建工作流编排层，结合记忆、消息传递和执行，以 Walrus 为底层存储
- 启用跨工具/跨 Agent 记忆共享，不同系统可读写存储在 Walrus 上的相同上下文
- 构建接口或开发者工具，方便检查、调试或管理存储在 Walrus 上的 Agent 记忆和数据

## 项目类型 / Project Types

你的项目可以是：
- 面向用户的 Agent 或多 Agent 系统
- 开发者工具或框架集成
- 与持久 AI 记忆和数据交互的新接口

## 我们在寻找什么 / What We're Looking For

不是演示——而是**可运行的系统**，展示：
- Agent 在能够记忆和持续构建时如何变得更有用
- 当数据可共享、持久、可移植时工作流如何改善
- 开发者如何超越脆弱的、孤岛化的记忆方案

## 资源 / Resources

- [Seal 文档](https://docs.sui.io) — Walrus 和 MemWal 的隐私层
- Sui Stack Messaging — 使用 Walrus 进行存储和恢复、使用 Seal 进行隐私保护的消息工具

## 学习资源 / Learning Resources

- [Let's Walrus](https://github.com/hoh-zone/lets-walrus) — Walrus 中文入门教程
- [Walrus 中文参考文档](https://hoh-zone.github.io/intro-walrus) — Walrus 概念与入门
