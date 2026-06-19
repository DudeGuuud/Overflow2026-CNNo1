# Contract Event Processor / 合约事件流处理器

> 实时处理 Predict 合约事件，转换为结构化数据供应用消费

## 解决什么问题

Predict 合约发出多种事件（OracleSVIUpdated、PositionMinted、PositionRedeemed 等），但原始 BCS 编码的事件数据难以直接使用。Contract Event Processor 提供实时事件流处理管道，将原始链上事件解码、转换、丰富为结构化数据，通过消息队列或 API 供下游应用消费，是 Predict 生态数据处理的基础组件。

## 核心功能

- 实时事件解码：将 BCS 编码的 Predict 事件解码为 JSON
- 事件丰富：关联交易上下文（发送者、Gas、时间戳）
- 多输出格式：Kafka/Webhook/REST API/GraphQL
- 事件聚合：按时间窗口聚合统计（交易量、活跃地址、结算数）

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| Sui Event API | 原始事件数据来源 |
| BCS 解码 | 将二进制事件转为结构化数据 |
| Predict 合约 ABI | 事件类型定义和字段映射 |
| Sui Checkpoint | 确保事件处理的有序性 |

## 实现方案

### Bot / Keeper / 服务端
- 事件订阅器：通过 Sui Event API 持续订阅 Predict 事件
- BCS 解码器：根据合约 ABI 将二进制数据解码为 JSON
- 事件丰富器：关联区块、发送者、Gas 等上下文信息
- 转换管道：可配置的转换规则（过滤、映射、聚合）
- 输出适配器：Kafka Producer / Webhook Poster / REST API
- 检查点管理器：记录已处理事件的检查点，支持断点续传

### 开发者工具
- npm/pip 包：`@predict/event-processor`
- 配置文件：YAML 定义事件过滤和转换规则
- CLI 工具：命令行启动处理器和查看状态
- 监控面板：处理速率、延迟、错误率

## 技术架构

Sui Event API 推送原始事件 → 事件订阅器接收 → BCS 解码器转为 JSON → 事件丰富器添加上下文 → 转换管道执行过滤/映射/聚合 → 输出适配器分发到 Kafka/Webhook/API → 检查点管理器确保不丢失。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 需要理解合约事件结构 |
| SDK 设计 | ⭐⭐⭐ | BCS 解码和管道设计 |
| 流处理 | ⭐⭐⭐⭐ | 高吞吐事件处理和检查点 |
| 集成复杂度 | ⭐⭐⭐ | 多输出格式和可靠性保证 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
