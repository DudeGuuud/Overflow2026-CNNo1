# Settled-Redeem Keeper / 结算赎回 Keeper 网络

> 监控已结算预言机，扫描未赎回仓位，调用 redeem_permissionless 赚小费

## 解决什么问题

Predict 合约结算后，许多用户不会立即赎回仓位，导致资金长时间锁定在合约中。虽然 predict::redeem_permissionless 允许任何人代为赎回，但缺乏主动扫描和执行的 Keeper 网络。Settled-Redeem Keeper 持续监控已结算预言机，发现未赎回仓位后代为赎回并收取小费，加速资金回笼。

## 核心功能

- 实时监控已结算预言机事件，扫描所有未赎回仓位和区间
- 自动调用 predict::redeem_permissionless 代为赎回
- 从赔付中抽取可配置比例的小费作为 Keeper 收益
- 多 Keeper 协调避免重复赎回（链上乐观锁 + 链下协调）

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::redeem_permissionless | 核心调用，代为赎回已结算仓位 |
| OracleSVIUpdated 事件 | 监控预言机结算状态 |
| PredictManager | 识别未赎回仓位 |
| PTB | 批量赎回多个仓位，降低 Gas |

## 实现方案

### 智能合约 (Move)
- `keeper_registry` 模块：Keeper 注册、小费配置、绩效追踪
- `redeem_tracker` 模块：记录已赎回仓位防止重复
- 关键数据结构：`KeeperState`（注册状态、小费比例、累计收益）、`RedeemRecord`（仓位 ID、赎回时间、小费金额）

### Bot / Keeper / 服务端
- 结算监听器：订阅预言机结算事件
- 未赎回扫描器：对比链上仓位状态与结算时间
- 批量赎回器：将多个赎回请求打包为一个 PTB
- 小费处理器：赎回后自动抽取小费到 Keeper 账户
- 碰撞避免：链下协调协议，多 Keeper 分区扫描

### 前端 / 后端
- 技术栈：React + Node.js + Redis
- Keeper 面板：已赎回仓位数量、累计小费收入
- 未赎回队列：当前等待赎回的仓位列表
- 绩效仪表盘：平均响应时间、Gas 效率

## 技术架构

结算监听器检测预言机结算 → 未赎回扫描器遍历链上仓位 → 批量赎回器打包多个 redeem_permissionless 到 PTB → 链上执行赎回并抽取小费 → 前端展示 Keeper 绩效和收益。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | Keeper 注册和小费逻辑 |
| Bot/算法 | ⭐⭐⭐ | 扫描和批量赎回逻辑 |
| 前端 | ⭐⭐ | Keeper 监控面板 |
| 集成复杂度 | ⭐⭐⭐ | 多 Keeper 协调是难点 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
