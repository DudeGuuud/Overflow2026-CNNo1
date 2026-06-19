# Oracle Health Monitor / 预言机健康监控

> 实时监控 OracleSVIUpdated 事件频率和延迟，异常时告警

## 解决什么问题

Predict 的预言机是整个协议的数据基石，OracleSVI 更新的及时性和准确性直接影响定价和结算质量。目前缺少专门监控预言机健康状况的工具。Oracle Health Monitor 实时追踪 OracleSVIUpdated 事件的频率、延迟和数据质量，当更新延迟过高或数据异常时立即告警，帮助协议参与者和开发团队快速发现问题。

## 核心功能

- 实时追踪 OracleSVIUpdated 事件频率和更新间隔
- 对比链上预言机价格与 Binance 实时价格的偏差
- 异常检测：更新延迟超过阈值、SVI 参数突变、数据缺失时告警
- 历史统计：平均更新间隔、最大延迟、数据可用率

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| OracleSVIUpdated 事件 | 核心监控对象 |
| predict-server API | 获取预言机历史数据 |
| Sui Event API | 实时订阅链上事件流 |
| OracleSVI | 解析和验证 SVI 参数合理性 |

## 实现方案

### Bot / Keeper / 服务端
- 事件订阅器：通过 Sui Event API 实时监听 OracleSVIUpdated
- 延迟计算器：测量事件时间戳与当前时间的偏差
- 偏差分析器：对比链上 SVI 价格与 Binance 实时 BTC 价格
- 异常检测引擎：基于统计阈值和 ML 模型检测异常模式
- 告警分发器：Webhook/Telegram/Discord 多通道告警

### 前端 / 后端
- 技术栈：React + Grafana + Prometheus + Node.js
- 实时监控仪表盘：更新频率、延迟分布、价格偏差
- 历史趋势图：更新间隔趋势、异常事件时间线
- 告警配置面板：自定义阈值和通知渠道

## 技术架构

Sui Event API 推送 OracleSVIUpdated → 事件订阅器捕获 → 延迟计算器和偏差分析器并行处理 → 异常检测引擎评估 → 超过阈值时告警分发器通知 → Prometheus 存储指标 → Grafana 展示监控面板。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐ | 无需自定义合约 |
| Bot/算法 | ⭐⭐⭐ | 异常检测算法和实时监控 |
| 前端 | ⭐⭐⭐ | 监控仪表盘和告警系统 |
| 集成复杂度 | ⭐⭐ | 主要是事件监听和数据分析 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
