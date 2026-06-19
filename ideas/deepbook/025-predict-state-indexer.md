# Predict State Indexer / Predict 状态索引器

> 高性能索引所有 Predict 事件，提供 REST/WS API 供其他应用使用

## 解决什么问题

构建 Predict 生态应用需要高效的链上数据访问，但直接从 Sui 全节点查询事件和状态效率低下。Predict State Indexer 提供高性能的链下索引服务，实时捕获并解析所有 Predict 相关事件，通过 REST API 和 WebSocket 向下游应用提供结构化数据查询。这是整个 Predict 生态的基础设施层，支撑金库、Bot、前端等所有应用。

## 核心功能

- 实时索引所有 Predict 合约事件（mint、redeem、supply、OracleSVIUpdated 等）
- 提供 REST API 查询仓位、交易历史、预言机状态
- WebSocket 实时推送事件流，支持过滤和订阅
- 聚合数据接口：交易者统计、市场概况、资金费率

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| Sui Event API | 捕获所有 Predict 合约事件 |
| Sui RPC | 读取链上状态和对象 |
| PredictManager | 解析仓位和管理器状态 |
| OracleSVI | 索引预言机更新历史 |

## 实现方案

### Bot / Keeper / 服务端
- 事件处理器：解析并验证每个 Predict 事件的 BCS 编码数据
- 状态管理器：维护内存中的 Predict 全状态（仓位、余额、预言机）
- 存储层：PostgreSQL 持久化 + Redis 缓存热数据
- API 网关：REST 端点 + WebSocket 连接管理
- 数据聚合器：定时计算交易量、活跃用户、资金费率等指标

### 前端 / 后端
- 技术栈：TypeScript + PostgreSQL + Redis + Docker
- API 文档：OpenAPI/Swagger 规范
- 健康检查面板：索引延迟、事件处理速率、存储状态
- 监控告警：索引延迟过高时通知

## 技术架构

Sui 全节点推送事件 → 事件处理器解析 BCS 数据 → 状态管理器更新内存索引 → 存储层写入 PostgreSQL → API 网关提供 REST/WS 查询 → 下游应用通过 API 获取数据。支持水平扩展和高可用部署。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐ | 无需自定义合约 |
| Bot/算法 | ⭐⭐⭐⭐ | BCS 解析和高性能索引 |
| 前端 | ⭐⭐ | API 文档和健康检查面板 |
| 集成复杂度 | ⭐⭐⭐⭐ | 需要处理所有 Predict 事件类型 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
