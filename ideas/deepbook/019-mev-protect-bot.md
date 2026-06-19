# MEV Protect Bot / MEV 保护交易机器人

> 通过 PTB 将多步操作打包为原子交易，保护 Predict 交易免受 MEV 攻击

## 解决什么问题

在 Sui 上执行 Predict 交易时，多步操作（批准 → 转账 → mint → 配置）之间存在 MEV 攻击窗口，恶意验证者或搜索者可以在步骤之间插入交易进行抢跑或三明治攻击。MEV Protect Bot 将整个交易流程打包为一个 Programmable Transaction Block（PTB），确保所有步骤原子执行，消除中间状态被利用的可能。

## 核心功能

- 将 dUSDC 批准、predict::mint、仓位配置打包为单一 PTB
- 支持 DeepBook 现货换 dUSDC → Predict mint 的原子操作
- 滑点保护：设置最大可接受价格偏差，超出自动回滚
- 交易模拟：提交前 dry-run 验证 PTB 执行结果

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| Programmable Transaction Block | 核心能力，原子化多步操作 |
| predict::mint | 打包在 PTB 中的核心交易 |
| deepbook::swap | 现货换 dUSDC 打包在同一 PTB |
| PredictManager | PTB 中创建或引用管理器 |

## 实现方案

### Bot / Keeper / 服务端
- PTB 构建器：动态组装多步交易为单一 PTB
- 交易模拟器：提交前 dry-run 检查执行路径和 Gas 消耗
- 滑点保护器：模拟结果与预期对比，超出阈值拒绝提交
- Gas 估算器：准确预估 PTB Gas 成本
- 交易提交器：选择低拥堵时段提交，或通过 Mysticeti 优先 Gas 拍速

### 前端 / 后端
- 技术栈：React + TypeScript + @mysten/sui.js
- PTB 可视化编辑器：拖拽组装交易步骤
- 交易模拟结果展示：预计输出、Gas 消耗
- 保护状态面板：已保护交易记录和 MEV 避免金额

## 技术架构

用户设定交易意图 → PTB 构建器将批准/转账/mint/配置打包为单一 PTB → 交易模拟器 dry-run 验证 → 滑点保护器对比预期结果 → 提交至 Sui 网络原子执行 → 前端展示保护状态。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 可能需要辅助模块 |
| Bot/算法 | ⭐⭐⭐ | PTB 构建和模拟逻辑 |
| 前端 | ⭐⭐⭐ | PTB 可视化编辑器 |
| 集成复杂度 | ⭐⭐⭐ | 深度理解 PTB 和交易构造 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
