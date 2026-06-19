# Trading Monitor Agent / 交易监控智能体

> 24/7 监控链上交易，交易历史和分析报告持久化在 Walrus，策略跨会话持续优化 / 24/7 on-chain transaction monitoring with persistent analysis in Walrus and cross-session strategy optimization

## 解决什么问题

交易者需要全天候监控链上动态，但现有工具缺乏 AI 驱动的智能分析和长期记忆。交易策略的优化需要历史数据的积累和回溯，而传统方案的数据容易丢失或无法跨平台同步。Walrus 提供不可篡改的交易记录存储，确保分析结果可验证。

## 核心功能

- 实时链上交易监控：通过 Sui RPC 订阅指定地址或协议的交易事件
- AI 分析报告生成：每 6 小时生成交易分析报告，包含趋势、异常、建议
- 策略持久化与优化：将策略参数和历史执行结果存储在 Walrus，Agent 基于历史表现优化
- 异常告警：检测到大额转账、异常模式时实时通知用户

## 使用的 Walrus / Sui 能力

| 能力 | 用途 |
|------|------|
| Walrus Blob 存储 | 存储交易分析报告（JSON）、策略配置文件、历史交易快照 |
| MemWal 记忆层 | 保存 Agent 对市场状态的认知和策略偏好，跨会话持续演进 |
| Sui 事件订阅 | 实时监听链上交易事件，触发 Agent 分析 |
| Sui 对象模型 | 链上记录监控配置、策略版本、绩效指标 |

## 实现方案

### Agent / AI 部分
- 使用 CrewAI 构建多角色 Agent：数据收集 Agent、分析 Agent、策略 Agent
- LLM 选用 GPT-4o 进行复杂分析，GPT-4o-mini 做快速分类
- 技术分析库：TA-Lib 计算 RSI、MACD 等指标
- 告警推送集成 Telegram Bot API

### Walrus 集成
- 分析报告以 `reports/{address}/{timestamp}.json` 存储
- 策略文件以 `strategies/{strategy_id}/v{n}.json` 版本化管理
- 每 24 小时将 MemWal 中的关键记忆点压缩并存储为 Blob
- 读取时按时间范围查询链上对象获取 Blob ID 列表

### 前端 / 后端
- 前端：React + ECharts 仪表盘，展示交易图表和策略表现
- 后端：Python FastAPI，WebSocket 推送实时监控数据
- 关键页面：实时监控仪表盘、策略管理页、历史报告页

## 技术架构

Sui 链上事件 → WebSocket 监听服务 → 数据收集 Agent 解析交易 → 分析 Agent 生成报告 → 报告写入 Walrus Blob → Blob ID 记录到 Sui 对象 → 前端通过 API 查询链上对象列表 → 从 Walrus 获取报告内容渲染。策略 Agent 定期读取历史 Blob 进行策略优化。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Walrus 集成 | ⭐⭐ | 结构化报告和策略文件的读写 |
| AI/Agent | ⭐⭐⭐⭐ | 实时分析、策略优化需要复杂推理 |
| 前端 | ⭐⭐⭐ | 实时仪表盘和图表展示 |
| 集成复杂度 | ⭐⭐⭐⭐ | WebSocket + 定时调度 + 多 Agent 协调 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
