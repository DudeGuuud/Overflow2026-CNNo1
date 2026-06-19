# Sui Dev Copilot / Sui 开发 Copilot

> VS Code 插件，AI 辅助编写 Move 合约，自动补全 Sui 特有模式（对象、能力、PTB）

## 解决什么问题

Move 语言的学习曲线陡峭，Sui 特有的对象模型、能力系统和 PTB 编程范式对新手不友好。现有通用代码补全工具不理解 Move 语法和 Sui 特有模式，经常生成无效代码。本项目提供 VS Code 插件，AI 深度理解 Sui Move 开发范式，辅助开发者编写合约、调试错误和生成 PTB 调用代码，加速 Sui 生态开发效率。

## 核心功能

- Move 代码智能补全：自动补全对象声明、能力约束、入口函数签名等 Sui 特有模式
- 错误诊断与修复：AI 分析编译错误，提供自然语言解释和一键修复建议
- PTB 调用代码生成：根据 Move 合约接口自动生成 TypeScript/Python PTB 调用代码

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | 插件理解对象声明、能力约束、所有权转移，辅助代码生成 |
| PTB | 根据 Move 接口自动生成 PTB 调用代码模板 |
| 链上数据 | 读取已部署 Package 的 ABI，辅助接口补全和调用代码生成 |
| 事件（Event） | 建议开发者在合约中添加事件的代码片段 |

## 实现方案

### 智能合约 (Move)
- **示例合约集**：提供涵盖常见模式（Token、NFT、DeFi、DAO）的 Move 合约模板库
- **`snippet_registry` 模块**：链上注册代码片段元数据，支持社区贡献和版本管理
- **`dev_profile` 模块**：`DevProfile` 对象记录开发者偏好、常用模式和贡献积分
- 关键数据结构：`SnippetMeta { name: String, category: u8, tags: vector<String>, rating: u64 }`

### 前端 / 后端
- **前端**：VS Code Extension API，Language Server Protocol (LSP) 实现语法高亮和补全
- **后端**：TypeScript LSP Server + Python AI 服务，代码分析、补全生成、错误诊断
- 关键功能：内联补全、悬浮文档、错误灯泡修复、PTB 代码生成面板

### AI / Agent 部分
- 代码补全：Fine-tuned Code 模型基于 Sui Move 代码库训练，理解对象和能力模式
- 错误诊断：GPT-4o 分析编译器错误信息，结合代码上下文生成修复方案
- PTB 生成：解析 Move 函数签名和对象依赖，自动生成完整的 PTB 调用代码
- Agent 工作流：编辑器事件 → 代码分析 → 上下文构建 → AI 推理 → 补全/修复建议 → 插入编辑器

## 技术架构

VS Code 插件通过 LSP Server 与 AI 后端通信。用户编辑 Move 代码时，LSP Server 收集上下文（当前文件、导入、项目结构），发送至 AI 服务。Fine-tuned 模型生成 Sui 特有模式的补全建议。编译错误触发 AI 诊断流程，GPT-4o 分析错误并结合 Sui 文档生成修复方案。PTB 生成器解析 Move 接口，自动生成类型安全的调用代码。链上 Snippet Registry 支持社区共享代码模式。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 模板合约和片段注册 |
| 前端 | ⭐⭐⭐⭐ | VS Code 扩展、LSP 实现和编辑器集成 |
| AI/ML | ⭐⭐⭐⭐ | Move 代码理解和补全模型训练 |
| 集成复杂度 | ⭐⭐⭐⭐ | LSP 协议、编辑器 API 和 AI 服务集成 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
