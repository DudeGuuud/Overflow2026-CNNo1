# Event Monitor Framework / 事件监控与告警框架

> 开发者自定义规则，AI Agent 监控链上事件并触发告警或自动响应

## 解决什么问题

Sui 链上事件是协议状态变更的关键信号，但开发者缺乏灵活的事件监控和响应工具。现有方案需要自行搭建索引服务，规则硬编码，无法适应复杂的事件模式匹配和智能化响应。本项目提供可扩展的事件监控框架，开发者通过自然语言或规则 DSL 定义监控条件，AI Agent 实时监听链上事件，匹配条件后触发告警或执行自动响应 PTB。

## 核心功能

- 自然语言定义监控规则：开发者描述「当 Scallop 借贷利用率超过 90% 时告警」，AI 解析为精确事件过滤条件
- 实时监听链上事件流，支持多条件组合、时间窗口聚合和阈值判断
- 触发后自动执行响应动作：发送告警（Webhook/Telegram）、构造 PTB 链上操作、调用外部 API

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| 事件（Event） | 核心数据源，订阅 Sui 链上事件流作为监控输入 |
| PTB | 告警触发后自动构造响应 PTB（如调整参数、暂停操作、转移资产） |
| Move 对象模型 | `MonitorRule` 对象存储监控规则，`AlertLog` 记录告警历史 |
| 共享对象 | `MonitorRule` 共享对象允许 DAO 或团队协作管理监控规则 |

## 实现方案

### 智能合约 (Move)
- **`monitor_rule` 模块**：定义 `MonitorRule` 对象，字段包括 `name: String`、`event_filter: vector<u8>`、`condition: vector<u8>`、`action_type: u8`、`threshold: u64`、`active: bool`、`creator: address`
- **`alert_log` 模块**：`log_alert(rule: &MonitorRule, trigger_event: vector<u8>, action_taken: u8, timestamp: u64)` 记录告警
- **`rule_registry` 模块**：`register_rule(name: String, filter: vector<u8>, condition: vector<u8>): MonitorRule` 注册新规则
- 关键数据结构：`AlertEntry { rule_id: ID, event_data: vector<u8>, severity: u8, action_result: bool, timestamp: u64 }`

### 前端 / 后端
- **前端**：React + Ant Design，规则编辑器、告警看板、事件流可视化、响应配置面板
- **后端**：Rust Agent 服务，事件订阅引擎、规则匹配器、动作执行器
- 关键功能：自然语言规则输入、事件流实时展示、告警历史、Webhook/通知配置

### AI / Agent 部分
- 规则解析：GPT-4o 将自然语言描述转换为结构化事件过滤条件和阈值判断逻辑
- 模式识别：AI 检测异常事件模式（如短时间内大量清算事件），触发未预见规则
- 告警分级：根据事件频率、影响范围和历史模式，AI 自动评估告警严重度
- Agent 工作流：事件订阅 → 规则匹配 → 条件评估 → 严重度分级 → 动作执行 → 日志记录

## 技术架构

Agent 服务通过 Sui WebSocket 订阅全链事件流。规则引擎加载 `MonitorRule` 对象中注册的监控条件，对每个事件进行多规则并行匹配。匹配成功后 AI 评估条件是否满足（如阈值比较、时间窗口聚合）。满足条件时触发响应动作：发送 Webhook/Telegram 告警、构造响应 PTB 提交链上、调用外部 API。所有告警记录写入 `AlertLog` 链上对象。前端提供规则编辑器和实时告警看板。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 规则注册和告警日志存储 |
| 前端 | ⭐⭐⭐ | 规则编辑器和告警看板 |
| AI/ML | ⭐⭐⭐ | 自然语言规则解析和异常检测 |
| 集成复杂度 | ⭐⭐⭐⭐ | 全链事件订阅、规则引擎和多通道告警 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
