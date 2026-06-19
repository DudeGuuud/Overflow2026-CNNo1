# DeepBook

**DeepBook 链上预测市场**

## Problem Statement / 核心命题

Prediction markets today are powerful, but still fundamentally fragmented and shallow. Most live venues either run as CLOB-matched event markets or as off-chain sportsbooks pretending to be markets. They settle slowly, list a narrow set of binary outcomes, can't price strikes or ranges, and have no real notion of an underlying volatility surface — making serious quant strategies, structured products, and on-chain risk transfer nearly impossible.

预测市场虽然强大，但本质上仍然碎片化和浅薄。大多数现有平台要么是 CLOB 匹配的事件市场（如 Polymarket），要么是伪装成市场的链下体育博彩。它们结算慢、只列出有限的二元结果、无法对行权价或区间定价、没有真正的波动率曲面概念——使得严肃的量化策略、结构化产品和链上风险转移几乎不可能。

随着预测市场从"X 会不会发生"的博彩演变为加密市场结构的真正组成部分，它们需要更坚实的基础：
- 能够根据实时波动率曲面定价**每个**行权价和到期日
- 短周期滚动到期，像真正的期权市场一样——亚小时周期，而非周度
- 一个接受每笔交易对手方的金库，使流动性始终存在，链上 LP 经济人人可审计和组合
- 可在更广泛的 Sui DeFi 生态中移植的原语——与保证金、借贷、结构化金库和机器人可组合

**本赛道挑战你围绕 DeepBook Predict——Sui 上的可编程、波动率曲面定价预测协议——构建创新产品和工具。**

## 当前状态 / Where Predict Is Today

- Predict 协议已在 **Sui 测试网**上线，支持滚动亚小时 BTC 预言机
- 公共索引器/API：`predict-server.testnet.mystenlabs.com`
- 报价资产：`dUSDC`
- 主网启动计划中，黑客松期间构建的项目预计在主网上线首日重新部署
- DeepBook 现货、`deepbook_margin`（保证金交易+清算）、`iron_bank`（许可 USDsui 供应）已在 Sui 主网运行

## Prize / 奖金

| Place | Prize |
|-------|-------|
| 1st | $35,000 |
| 2nd | $15,000 |
| 3rd | $7,500 |
| 4th | $5,000 |

## 参考方向 / Suggested Topics

选一个、改编一个、或者完全忽略——这些只是起点。

### 1. Range Ladder Vault（阶梯区间金库）

自动将用户资金存入每个新到期日围绕平值行权价的一系列 Predict 区间，结算时自动滚动到下一个到期日。发行代币化份额，使仓位可在 Sui DeFi 中移植。选择行权宽度策略（固定基点、SVI 1σ、基于已实现波动率动态调整），处理深度实值/虚值滚动，暴露提取队列。

### 2. PLP + Hedge Vault（PLP + 对冲金库）

向 `predict::supply` 供应报价资产赚取 PLP 收益，同时通过 `predict::mint` 购买虚值二元期权限制左尾回撤。产品是"PLP 收益减去崩盘保险"——比纯 PLP 更容易向外部 LP 推销。动态调整对冲比例，接近到期时卖回对冲，展示扣除保险成本后的净 APY。

### 3. BTC-collateral Predict Vault（BTC 抵押预测金库）

接受 BTC（xBTC、sBTC 等），通过 DeepBook 现货转换为 dUSDC，存入 `PredictManager`，运行方向性或区间策略，向用户返回 BTC 计价的收益。选择策略（Delta 中性权利金收割 vs 方向性动量），诚实定价 FX 腿，处理结算日的换回。

### 4. Three-Protocol Margin Loop（三协议保证金循环）

在 `deepbook_margin` 上以 `iron_bank` USDsui 份额代币为抵押借入 dUSDC，部署到 Predict 区间，从结算赔付中偿还。一个团队，三个协议 logo——"这就是 Sui DeFi 可组合性真正样子"的旗舰演示。设计清算路径，以最坏情况 Predict 结果为边界限制 LTV，暴露一个打开整个堆栈的原子 PTB。

### 5. Telegram Quick-Predict Bot

`/up 70k 15m 100usdc` 等命令解析为 `predict::mint`，内联键盘提供"立即赎回 / 显示 PnL / 分享到群"，结算触发带结果的 DM。最低摩擦的非加密原生用户入门——Bot 首次使用时创建 `PredictManager`，后台自动获取 dUSDC。群聊锦标赛、跟单、排行榜。

### 6. Streaks & Leaderboard PWA

每日二元选择（"BTC 收盘涨还是跌"），每用户连胜记录，每周奖池。基于 Predict mint/redeem 流程的游戏化留存循环。连胜 NFT 徽章、链上 Manager 间交互的社交图谱、移动优先安装流程。

### 7. Vol-Arb Bot: Predict ↔ Polymarket（波动率套利机器人）

从 `OracleSVI` 反推 Predict 的隐含波动率，与 Polymarket BTC 期权微笑曲线在匹配到期日比较，当超过阈值时交易价差。进阶：在 Hyperliquid 永续合约上 Delta 对冲二元期权，使 Bot PnL 为纯波动率边缘。优雅处理过时 SVI 更新，按 Kelly 比例下注，添加馈送延迟时的止损开关。

### 8. Settled-Redeem Keeper Network（结算赎回 Keeper 网络）

监控已结算预言机，扫描索引器中的未赎回仓位和区间，在单个 PTB 中调用 `predict::redeem_permissionless` 代表所有者领取赔付——从赔付中分取小费。多 Keeper 协调避免碰撞，抗 MEV 提交，选择加入/退出的小费策略。

### 9. Predict Surface Studio（波动率曲面工作室）

从 `oracle::OracleSVIUpdated` 事件流式传输的实时 3D 波动率曲面（行权价 × 到期日 → 隐含波动率），带时间旅行滑块回放近期更新，以及无套利检查器标记蝶式或日历违规。与 Polymarket 微笑曲线并排比较，微笑形态变化警报，可嵌入其他 Sui 前端的组件。

### 10. PLP Risk Dashboard（PLP 风险仪表盘）

金库利用率、提取限制器令牌桶状态、每预言机风险敞口分解，以及"假设"场景模拟器展示 ±5σ BTC 移动下的 PLP PnL。历史回撤回放、每行权价金库库存热力图、面向机构 LP 的可导出风险报告。

## 合格要求 / Qualification Requirements

- 集成 **DeepBook Predict 合约**（测试网）
- 构建产品的话，**端到端可用**——会测试整个流程
- 构建金库策略的话，需要**合适的模拟结果**

## 项目类型 / Project Types

你的项目可以是：
- 面向用户的应用或交易前端
- 基于 Predict 构建的金库、结构化产品或可组合代币
- Bot、Keeper 或套利服务
- 开发者工具、SDK 或分析仪表盘

## 学习资源 / Learning Resources

- [DeepBook Bootcamp](https://github.com/hoh-zone/deepbook-bootcamp) — DeepBook 中文实战教程
- [DeepBook 中文参考文档](https://hoh-zone.github.io/intro-deepbook) — DeepBook 概念与入门
