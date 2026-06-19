# Telegram Quick-Predict Bot / Telegram 快捷预测机器人

> /up 70k 15m 100usdc 命令直接下单，结算后 DM 通知结果

## 解决什么问题

Predict 的链上交易流程对非加密原生用户门槛过高：需要安装钱包、获取 dUSDC、理解链上交互。Telegram Quick-Predict Bot 提供最低摩擦的用户入门体验——用户只需发送简单命令即可下单，Bot 后台自动处理钱包创建、dUSDC 获取、合约交互。结算后通过 DM 通知结果，实现零 Web3 知识的预测交易。

## 核心功能

- 自然语言命令解析：`/up 70k 15m 100usdc` 或 `/down 65k 30m 50`
- 首次使用自动创建 PredictManager 和获取 dUSDC
- 内联键盘：一键赎回、查看 PnL、分享到群
- 结算 DM 通知：自动推送交易结果和盈亏

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::mint | 解析命令后执行交易 |
| predict::redeem_permissionless | 到期后自动赎回 |
| PredictManager | 首次使用自动创建管理器 |
| dUSDC | 报价资产，Bot 后台自动获取 |

## 实现方案

### Bot / Keeper / 服务端
- 命令解析器：自然语言命令转为结构化交易参数
- 钱包管理器：为每个 Telegram 用户创建 Sui 钱包
- 交易执行器：通过 @mysten/sui.js 调用 predict::mint
- 结算监控器：追踪用户仓位到期并赎回
- 通知引擎：Telegram Bot API 推送结算结果

### 前端 / 后端
- 技术栈：Node.js + grammy/telegraf + PostgreSQL
- Telegram Bot 界面：命令帮助、内联键盘、结果卡片
- Web Dashboard：账户余额、交易历史、盈亏统计
- 管理面板：用户统计、Bot 运行状态

## 技术架构

用户发送 Telegram 命令 → 命令解析器提取交易参数 → 检查/创建用户钱包 → 获取/补充 dUSDC → 交易执行器调用 predict::mint → 结算监控器等待到期 → 自动 redeem → 通知引擎推送 DM 结果。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐ | 无需自定义合约 |
| Bot/算法 | ⭐⭐⭐ | 命令解析和钱包管理 |
| 前端 | ⭐⭐⭐ | Telegram Bot UX 和 Web Dashboard |
| 集成复杂度 | ⭐⭐⭐ | 自动钱包和 dUSDC 管理 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
