# SVI Curve Comparer / SVI 曲线对比器

> 并排比较 Predict SVI 与 Polymarket/Hyperliquid 的波动率微笑

## 解决什么问题

Predict 使用 SVI 模型对 BTC 预测定价，但交易者无法直观比较 Predict 的波动率微笑与外部市场（Polymarket、Hyperliquid）的差异。SVI Curve Comparer 并排展示各平台的波动率微笑曲线，帮助套利者发现定价偏差、研究者理解市场微观结构、做市商校准报价。

## 核心功能

- 多平台波动率微笑并排对比（Predict / Polymarket / Hyperliquid）
- 实时更新：Predict SVI 变化时自动刷新曲线
- 偏差分析：量化各平台间的 IV 差异和套利空间
- 历史对比：回放不同时间点的跨平台微笑曲线差异

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| OracleSVI | 核心数据源，获取 Predict 波动率参数 |
| OracleSVIUpdated 事件 | 实时更新微笑曲线 |
| predict-server API | 获取历史 SVI 数据 |

## 实现方案

### Bot / Keeper / 服务端
- SVI 解析器：从 a, b, rho, m, sigma 参数计算各行权价 IV
- 外部数据采集器：通过 API 获取 Polymarket 和 Hyperliquid 期权数据
- 波动率反推器：从外部平台期权价格反推 IV
- 偏差分析器：计算各平台 IV 差异的统计指标
- 历史存储：定期快照各平台微笑曲线

### 前端 / 后端
- 技术栈：React + D3.js + Python FastAPI
- 曲线对比面板：多平台微笑曲线叠加展示
- 偏差热力图：各行权价的跨平台 IV 差异
- 历史回放：时间轴滑块查看历史对比
- 导出功能：CSV/JSON 导出数据

## 技术架构

OracleSVI 提供链上波动率参数 → SVI 解析器计算各行权价 IV → 外部采集器获取 Polymarket/Hyperliquid 数据 → 波动率反推器计算外部 IV → 偏差分析器对比差异 → 前端并排展示曲线。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐ | 无需自定义合约 |
| Bot/算法 | ⭐⭐⭐⭐ | SVI 解析和外部 IV 反推 |
| 前端 | ⭐⭐⭐⭐ | 多曲线对比可视化 |
| 集成复杂度 | ⭐⭐⭐⭐ | 多外部平台数据集成 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
