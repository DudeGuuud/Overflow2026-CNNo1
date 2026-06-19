# Predict Surface Studio / 波动率曲面工作室

> 实时 3D 波动率曲面可视化（行权价 × 到期日 → IV），带时间旅行回放

## 解决什么问题

Predict 使用 SVI（Stochastic Volatility Inspired）模型对每个行权价和到期日定价，形成三维波动率曲面。但这个曲面在链上只是参数形式存储，缺少直观的可视化工具。Predict Surface Studio 将 OracleSVI 参数实时渲染为交互式 3D 波动率曲面，支持时间旅行回放历史变化，帮助交易者和研究者直观理解市场定价。

## 核心功能

- 实时 3D 波动率曲面渲染：行权价 × 到期日 → IV 的三维曲面
- 时间旅行滑块：回放近期 SVI 参数变化过程
- 无套利检查器：标记蝶式和日历价差违规区域
- 与 Polymarket 微笑曲线并排比较

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| OracleSVIUpdated 事件 | 获取实时 SVI 参数 |
| OracleSVI | 解析 a, b, rho, m, sigma 参数 |
| predict-server API | 获取历史 SVI 数据 |
| Sui Event API | 订阅 SVI 更新事件流 |

## 实现方案

### 前端 / 后端
- 技术栈：React + Three.js / Deck.gl + WebGPU
- SVI 解析器：将 a, b, rho, m, sigma 参数转为 IV 曲面
- 3D 渲染引擎：实时渲染波动率曲面网格
- 时间控制器：存储历史 SVI 快照，支持回放
- 无套利检查：验证蝶式不等式和日历单调性
- 对比模式：并排展示 Predict 和外部平台曲面

### Bot / Keeper / 服务端
- SVI 数据服务：实时订阅事件并存储历史快照
- 外部数据采集器：获取 Polymarket/Hyperliquid 波动率数据
- 无套利分析器：后台计算违规区域

## 技术架构

OracleSVIUpdated 事件推送 → SVI 解析器转换为 IV 曲面 → 3D 渲染引擎实时渲染 → 历史快照存储支持回放 → 无套利检查器标记异常 → 外部数据并排对比 → 用户交互式探索。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐ | 无需自定义合约 |
| Bot/算法 | ⭐⭐⭐ | SVI 解析和无套利检查 |
| 前端 | ⭐⭐⭐⭐⭐ | 3D 可视化和交互是核心 |
| 集成复杂度 | ⭐⭐⭐ | 数据采集和实时渲染 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
