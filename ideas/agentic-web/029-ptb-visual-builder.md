# PTB Visual Builder / PTB 可视化构建器

> 拖拽式界面构建 PTB，AI 辅助推荐下一步操作和风险提示

## 解决什么问题

Sui 的可编程交易块（PTB）是强大的原子化交易组合工具，但手动编写 PTB 需要深入理解 Sui SDK 和交易构造，学习曲线陡峭。开发者经常因参数不匹配或 Gas 估算错误导致交易失败。本项目提供可视化拖拽式 PTB 构建器，AI 辅助推荐下一步操作、自动校验参数兼容性，大幅降低 PTB 开发门槛。

## 核心功能

- 拖拽式节点编辑器构建 PTB 流程图，每个节点代表一个交易指令（拆币、合并、Move Call 等）
- AI 根据已有节点自动推荐下一步可执行操作，智能补全参数和类型约束
- 一键模拟执行 PTB，预览 Gas 消耗和状态变更，确认后直接提交上链

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| PTB | 核心构建目标，可视化生成 PTB 指令序列并提交执行 |
| Move 对象模型 | `TemplatePTB` 对象存储用户保存的 PTB 模板，支持分享和复用 |
| 链上数据 | 实时读取对象信息和类型定义，辅助参数校验和补全 |
| 事件（Event） | 发出 `PTBExecutedEvent` 记录模板执行历史和结果 |

## 实现方案

### 智能合约 (Move)
- **`ptb_template` 模块**：定义 `TemplatePTB` 对象，字段包括 `name: String`、`description: String`、`steps: vector<u8>`（序列化步骤）、`creator: address`、`use_count: u64`、`created_at: u64`
- **`template_share` 模块**：`share_template(template: TemplatePTB)` 将模板转为共享对象供他人使用
- **`execution_log` 模块**：`log_execution(template: &TemplatePTB, gas_used: u64, success: bool)` 记录执行结果
- 关键数据结构：`PTBStep { command: u8, type_args: vector<String>, args: vector<Argument>, gas_budget: u64 }`

### 前端 / 后端
- **前端**：React + React Flow（节点编辑器），拖拽画布、节点属性面板、Gas 预览、模板市场
- **后端**：TypeScript 服务，PTB 序列化/反序列化、Dry Run 模拟、类型推断引擎
- 关键功能：节点拖拽、参数校验、AI 推荐、模板保存/加载、一键执行

### AI / Agent 部分
- 上下文感知推荐：GPT-4o 根据已有 PTB 节点和用户意图，推荐下一步操作和参数
- 类型推断：自动推断 PTB 步骤间的类型传递关系，标记不兼容的连接
- 风险检测：分析 PTB 整体行为，检测潜在的无限循环、资金锁定等风险
- Agent 工作流：用户添加节点 → 类型推断 → 可行操作检索 → AI 排序推荐 → 参数补全 → Dry Run → 上链

## 技术架构

用户在前端节点编辑器中拖拽构建 PTB 流程图。类型推断引擎实时分析节点间类型兼容性，不匹配时高亮提示。GPT-4o 根据已有节点序列和用户目标推荐下一步操作。完成后用户点击「模拟执行」，后端调用 Sui Dry Run API 预览 Gas 和状态变更。确认后 PTB 直接提交链上执行。常用流程可保存为 `TemplatePTB` 对象分享至模板市场。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 模板存储和执行日志记录 |
| 前端 | ⭐⭐⭐⭐ | 节点编辑器、拖拽交互和类型可视化 |
| AI/ML | ⭐⭐⭐ | PTB 模式理解和操作推荐 |
| 集成复杂度 | ⭐⭐⭐⭐ | PTB SDK 集成、类型系统和 Dry Run |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
