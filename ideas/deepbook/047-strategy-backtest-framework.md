# Strategy Backtest Framework / 策略回测框架

> 基于历史预言机数据回测 Predict 策略，计算 Sharpe/最大回撤

## 解决什么问题

开发 Predict 交易策略时，开发者无法在真实市场中反复试错——成本太高且市场不会重演。Strategy Backtest Framework 提供基于历史预言机数据的回测环境，让开发者用历史数据验证策略逻辑，计算 Sharpe 比率、最大回撤、胜率等关键指标，在真实部署前充分评估策略表现。

## 核心功能

- 历史数据回放：基于历史 OracleSVI 和 BTC 价格回放市场
- 策略 API：定义策略接口，开发者只需实现 `on_tick` 和 `on_settle`
- 绩效指标：自动计算 Sharpe、Sortino、最大回撤、胜率、盈亏比
- 可视化报告：生成策略回测报告和净值曲线

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| OracleSVI 历史数据 | 回测环境的核心数据源 |
| predict::mint 逻辑 | 模拟 mint 操作和成本 |
| predict::redeem 逻辑 | 模拟结算和收益 |
| predict-server API | 获取历史数据 |

## 实现方案

### SDK 核心
- 数据加载器：从 predict-server 获取历史 OracleSVI 和价格数据
- 市场模拟器：模拟 Predict 合约的定价和结算逻辑
- 策略接口：定义 `Strategy` 基类（on_tick、on_settle、on_expire）
- 交易模拟器：模拟 mint/redeem 的成本和收益
- 绩效分析器：计算 Sharpe、Sortino、最大回撤等指标
- 报告生成器：生成交互式 HTML 回测报告

### 开发者工具
- 策略模板：常用策略的示例代码（动量、均值回归、做市）
- 参数优化器：网格搜索和随机搜索最优参数
- 对比工具：多策略并排对比
- Jupyter 集成：在 Notebook 中交互式回测

## 技术架构

数据加载器获取历史数据 → 市场模拟器按时间顺序回放 → 策略的 on_tick 在每个时间步执行 → 交易模拟器记录虚拟交易 → 结算时 on_settle 更新虚拟余额 → 绩效分析器计算指标 → 报告生成器输出结果。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐ | 无需自定义合约 |
| SDK 设计 | ⭐⭐⭐⭐ | 市场模拟和绩效分析 |
| 回测引擎 | ⭐⭐⭐⭐⭐ | 准确模拟合约逻辑是核心 |
| 集成复杂度 | ⭐⭐⭐ | 需要精确复现 Predict 定价逻辑 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
