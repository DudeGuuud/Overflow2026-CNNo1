# Research-Trading Dual Agent / 研究-交易双智能体

> 研究 Agent 分析市场生成报告，交易 Agent 读取报告执行策略，共享 Walrus 存储 / Research Agent analyzes markets and generates reports; Trading Agent reads reports and executes strategies via shared Walrus storage

## 解决什么问题

研究和交易是 DeFi 中紧密关联的两个环节，但现有系统通常将它们割裂。研究产生的洞察无法直接驱动交易执行，交易结果也无法反馈到研究中。通过双 Agent 协作和 Walrus 共享存储，实现从研究到交易的闭环自动化。

## 核心功能

- 研究 Agent 独立运行：持续分析市场数据、新闻、链上指标，输出研究报告
- 交易 Agent 读取执行：从 Walrus 读取最新研究报告，据此生成和执行交易策略
- 共享状态管理：两个 Agent 通过 Walrus 共享中间状态、报告、策略参数
- 绩效反馈循环：交易结果写回 Walrus，研究 Agent 下一轮分析时参考

## 使用的 Walrus / Sui 能力

| 能力 | 用途 |
|------|------|
| Walrus Blob 存储 | 存储 Agent 间共享的研究报告、策略文件、交易结果 |
| MemWal 记忆层 | 各 Agent 维护独立的记忆空间 + 共享的记忆通道 |
| Sui 对象模型 | 链上记录策略执行记录、权限控制（研究/交易角色） |
| Walrus 读取 API | Agent 间异步通信的读取接口 |

## 实现方案

### Agent / AI 部分
- 使用 AutoGen 构建双 Agent 系统，支持 Agent 间消息传递
- 研究 Agent：GPT-4o + 市场数据 API（CoinGecko、DefiLlama）
- 交易 Agent：GPT-4o + Sui TypeScript SDK 执行链上交易
- 协调器使用 LangGraph StateGraph 管理双 Agent 工作流

### Walrus 集成
- 研究报告以 `shared/research/{timestamp}.json` 存储
- 策略文件以 `shared/strategies/{strategy_id}.json` 存储
- 交易结果以 `shared/trades/{timestamp}.json` 存储
- 共享状态以 `shared/state/latest.json` 存储，Agent 各自读写
- Sui 链上 AgentRegistry 对象记录 Agent 身份和权限

### 前端 / 后端
- 前端：React + TradingView Widget，展示研究和交易双面板
- 后端：Python FastAPI，管理 Agent 生命周期和调度
- 关键页面：研究-交易双面板、策略配置页、绩效分析页、Agent 日志页

## 技术架构

研究 Agent 定时执行 → 市场数据分析 → 生成研究报告 → 写入 Walrus Blob → 通知交易 Agent → 交易 Agent 读取报告 → 生成策略 → 执行交易 → 交易结果写入 Walrus → 研究 Agent 下一轮读取交易结果优化分析 → 形成闭环。MemWal 管理各 Agent 的独立记忆和共享记忆。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Walrus 集成 | ⭐⭐⭐ | Agent 间共享存储需要并发控制 |
| AI/Agent | ⭐⭐⭐⭐ | 双 Agent 协调和策略自动化 |
| 前端 | ⭐⭐⭐ | 双面板实时展示 |
| 集成复杂度 | ⭐⭐⭐⭐⭐ | 多 Agent 通信 + 交易执行 + 状态同步 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
