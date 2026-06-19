# Three-Protocol Margin Loop / 三协议保证金循环

> iron_bank 借 → deepbook_margin 杠杆 → Predict 部署，一个 PTB 原子化打开全栈

## 解决什么问题

Sui DeFi 生态中各协议独立运作，用户需要手动在多个平台间搬运资金，效率低下且无法捕获组合收益。三协议保证金循环通过一个原子化 PTB 将 iron_bank 借贷、deepbook_margin 杠杆和 Predict 部署串联，展示 Sui DeFi 可组合性的真正威力。

## 核心功能

- 一个 PTB 原子化完成：iron_bank 供应 USDsui → deepbook_margin 以 USDsui 为抵押借入 dUSDC → Predict 部署区间
- 实时计算 LTV 和清算风险，以最坏情况 Predict 结果为边界
- 自动清算路径设计：Predict 结算赔付 → 偿还 margin 借款 → 归还 iron_bank
- 风险仪表盘展示三协议全栈敞口

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::mint | 在 Predict 上部署区间策略 |
| predict::redeem_permissionless | 结算后赎回赔付 |
| iron_bank | 供应 USDsui 赚取基础利息 |
| deepbook_margin | 以 USDsui 为抵押借入 dUSDC |
| PredictManager | 管理全部 Predict 仓位 |

## 实现方案

### 智能合约 (Move)
- `margin_loop` 模块：编排三协议 PTB、管理全局状态
- `risk_engine` 模块：实时计算 LTV、最大损失、清算边界
- 关键数据结构：`LoopPosition`（iron_bank 供应量、margin 借款量、Predict 仓位）、`RiskParams`（LTV 上限、清算阈值）

### Bot / Keeper / 服务端
- 监控任务：实时追踪三协议仓位健康度
- 清算保护：当 LTV 接近阈值时自动平仓部分 Predict 仓位
- 结算路由：Predict 结算后按优先级偿还债务

### 前端 / 后端
- 技术栈：React + Three.js（三协议可视化）
- 一键开启循环按钮，实时展示资金流向图
- 风险指标面板：LTV、清算价格、净 APY
- 压力测试模拟器

## 技术架构

用户点击"开启循环" → 前端构建 PTB：iron_bank::supply(USDsui) → margin::borrow(dUSDC, collateral=USDsui) → predict::mint(区间) → 一个原子交易提交。Keeper 持续监控仓位健康度，结算时 PTB 执行：redeem → repay_margin → 可选提取。前端实时展示全栈状态。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐⭐⭐ | 三协议 PTB 编排是最大挑战 |
| Bot/算法 | ⭐⭐⭐⭐ | 风险管理和清算路径复杂 |
| 前端 | ⭐⭐⭐ | 三协议资金流可视化 |
| 集成复杂度 | ⭐⭐⭐⭐⭐ | 需要深入理解三个协议的接口 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
