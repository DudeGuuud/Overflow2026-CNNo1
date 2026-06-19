# Discord Predict Bot / Discord 预测机器人

> 在 Discord 服务器中创建预测房间，成员可下注和查看排行

## 解决什么问题

加密社区大量活跃在 Discord 服务器中，但参与 Predict 交易需要离开 Discord 打开独立的 DApp。Discord Predict Bot 将预测市场直接带入 Discord 服务器，服务器管理员可以创建预测房间，成员使用斜杠命令下注和查看排行，实现社区内预测交易和互动。结算后自动更新排行榜，增强社区活跃度。

## 核心功能

- 斜杠命令交易：/predict up 70k 15m 100 下注
- 预测房间：服务器管理员创建专属预测频道
- 实时排行榜：服务器内成员盈亏和胜率排名
- 结算通知：自动推送结算结果到频道

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::mint | 解析 Discord 命令后执行交易 |
| predict::redeem_permissionless | 结算后自动赎回 |
| PredictManager | 管理 Discord 用户仓位 |
| OracleSVI | 获取实时价格展示在 Discord |

## 实现方案

### Bot / Keeper / 服务端
- Discord Bot：基于 discord.js 的命令处理和交互
- 命令解析器：/predict、/positions、/leaderboard 等命令
- 钱包管理器：Discord 用户绑定 Sui 钱包地址
- 交易执行器：调用 predict::mint 执行下注
- 结算监控器：追踪仓位到期并自动 redeem
- 排行榜引擎：计算服务器内排名并更新 Embed

### 前端 / 后端
- 技术栈：Node.js + discord.js + PostgreSQL
- Discord Bot 界面：Slash 命令、Embed 消息、Button 交互
- Web 管理面板：Bot 配置、服务器统计
- 钱包绑定页面：Discord OAuth + Sui 钱包连接

## 技术架构

Discord 用户发送 /predict 命令 → Bot 解析参数 → 检查用户钱包绑定 → 交易执行器调用 predict::mint → 发送确认 Embed → 结算监控器等待到期 → 自动 redeem → 更新排行榜 Embed → 推送结算结果。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐ | 无需自定义合约 |
| Bot/算法 | ⭐⭐⭐ | Discord 交互和钱包管理 |
| 前端 | ⭐⭐⭐ | Discord Embed 和 Web 管理面板 |
| 集成复杂度 | ⭐⭐⭐ | Discord API 和链上交互整合 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
