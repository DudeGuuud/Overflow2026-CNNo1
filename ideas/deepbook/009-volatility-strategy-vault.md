# Volatility Strategy Vault / 波动率策略金库

> 根据 SVI 曲线形态，低买高卖隐含波动率，跨到期日套利

## 解决什么问题

链上缺少专业的波动率交易工具，无法像传统金融市场那样进行波动率套利。波动率策略金库通过分析 OracleSVI 曲线形态，识别隐含波动率被低估或高估的区间，低买高卖，并在不同到期日之间进行日历套利。

## 核心功能

- 实时分析 OracleSVI 曲线，识别波动率异常（偏高/偏低）
- 低 IV 时买入区间，高 IV 时卖出区间
- 日历套利：近月低 IV + 远月高 IV 时构建价差
- 波动率均值回归策略：IV 偏离历史均值时自动交易

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::mint | 买入/卖出区间执行波动率策略 |
| predict::redeem_permissionless | 策略到期后赎回 |
| PredictManager | 管理所有波动率策略仓位 |
| OracleSVI | 核心数据源，读取实时和历史的 SVI 曲线 |

## 实现方案

### 智能合约 (Move)
- `vol_vault` 模块：存款管理、份额代币
- `vol_strategy` 模块：记录策略类型和参数
- 关键数据结构：`VolPosition`（行权价、到期日、IV 入场价、方向）、`StrategyConfig`（IV 阈值、最大敞口）

### Bot / Keeper / 服务端
- SVI 分析引擎：Python 服务，实时解析 SVI 曲线形态
- 信号生成器：当 IV 偏离阈值时生成交易信号
- 策略执行器：提交 PTB 执行 mint 操作
- 日历套利管理器：跟踪跨到期日的价差仓位

### 前端 / 后端
- 技术栈：React + Three.js（3D 波动率曲面）
- SVI 曲线实时展示和策略标注
- 策略 PnL 跟踪，IV 入场/出场对比

## 技术架构

Python 服务持续监听 OracleSVIUpdated 事件 → 分析 SVI 曲线形态 → 发现异常时生成信号 → 后端提交交易到合约 → predict::mint 执行策略 → 到期时 redeem → 前端实时展示 SVI 曲线和策略标记。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 标准金库结构 |
| Bot/算法 | ⭐⭐⭐⭐⭐ | SVI 分析和波动率套利策略是核心 |
| 前端 | ⭐⭐⭐⭐ | 3D 曲面可视化 |
| 集成复杂度 | ⭐⭐⭐ | 依赖 SVI 数据流 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
