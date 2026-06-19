# Kelly Position Bot / Kelly 仓位管理机器人

> 根据胜率计算 Kelly 比例，自动调整每笔 Predict 交易的仓位大小

## 解决什么问题

交易者在 Predict 市场上往往凭感觉决定仓位大小，容易过度下注导致爆仓或下注不足浪费机会。Kelly 公式提供数学最优的仓位管理方法，根据估计胜率和赔率计算最优下注比例。Kelly Position Bot 自动评估每笔交易的胜率，按 Kelly 比例调整仓位，实现长期资金最大化增长。

## 核心功能

- 基于 SVI 曲线和历史数据自动评估每笔交易的隐含胜率
- 按 Kelly 公式计算最优下注比例（支持半 Kelly / 四分之一 Kelly）
- 自动执行 predict::mint，按计算比例分配资金
- 连续交易后自动追踪真实胜率，动态校准模型

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::mint | 按最优仓位比例执行交易 |
| predict::redeem_permissionless | 到期后赎回结算资金 |
| OracleSVI | 评估隐含概率作为 Kelly 输入 |
| PredictManager | 管理资金池和仓位记录 |

## 实现方案

### Bot / Keeper / 服务端
- 胜率估计器：结合 SVI 隐含概率、历史预测准确率、市场微观结构
- Kelly 计算器：f* = (bp - q) / b，支持分 Kelly 调整
- 仓位执行器：按 Kelly 比例计算 dUSDC 数量，调用 predict::mint
- 胜率追踪器：记录每笔交易结果，滚动更新历史胜率
- 回撤保护：连续亏损时自动降低 Kelly 系数

### 前端 / 后端
- 技术栈：React + Recharts + Node.js
- Kelly 仓位面板：当前仓位、Kelly 比例、资金增长曲线
- 胜率校准图表：预测胜率 vs 实际胜率对比
- 风险指标：最大回撤、Sharpe 比率、资金利用率

## 技术架构

市场数据采集 → 胜率估计器计算每笔交易预期胜率 → Kelly 计算器得出最优仓位比例 → 仓位执行器调用 predict::mint 下单 → 结算后 redeem 并更新胜率记录 → 前端展示资金增长曲线和风险指标。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐ | 无需自定义合约 |
| Bot/算法 | ⭐⭐⭐⭐ | 胜率估计和 Kelly 动态调整是核心 |
| 前端 | ⭐⭐⭐ | 资金曲线和风险指标可视化 |
| 集成复杂度 | ⭐⭐⭐ | 需要持续追踪和校准胜率 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
