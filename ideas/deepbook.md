# DeepBook - 50 Project Ideas

基于官方 [Problem Statement](https://mystenlabs.notion.site/deepbook-predict-problem-statement)，项目需集成 DeepBook Predict 合约（测试网），围绕预测市场构建应用、工具、金库或 Bot。Predict 已支持滚动亚小时 BTC 预言机、SVI 波动率曲面、`dUSDC` 报价。

---

## 金库与结构化产品

1. **Range Ladder Vault** — 自动在 ATM 行权价周围存入一列区间，到期自动滚动到下一期，发行代币化份额
2. **PLP + 对冲金库** — 供应 `dUSDC` 赚取 PLP 收益，同时买入虚值二元期权对冲尾部风险
3. **BTC 抵押预测金库** — 接受 BTC 抵押，通过 DeepBook 现货换成 `dUSDC`，运行方向性策略，归还 BTC 收益
4. **三协议保证金循环** — `iron_bank` 借 → `deepbook_margin` 杠杆 → Predict 部署，一个 PTB 原子化打开全栈
5. **Delta 中立权利金收割** — 持续卖出区间期权，收取权利金，通过 DeepBook 现货 Delta 对冲
6. **动量策略金库** — AI 判断 BTC 方向，自动买入对应方向二元期权，止盈止损自动执行
7. **网格交易金库** — 在 Predict 区间中部署价格网格，每个区间独立管理，到期自动重新部署
8. **稳定币增强金库** — USDC 存入 PLP 赚取基础收益，收益部分自动投入低风险区间策略增强回报
9. **波动率策略金库** — 根据 SVI 曲线形态，低买高卖隐含波动率，跨到期日套利
10. **保险封装金库** — 包装 Predict 二元期权为"价格保险"产品，用户买"BTC 不会跌破 X"的保单

## 交易机器人

11. **Vol-Arb Bot：Predict ↔ Polymarket** — 反推 Predict 隐含波动率与 Polymarket 比较，超过阈值时交易价差
12. **Delta 对冲 Bot** — 在 Predict 上卖出期权的同时在 Hyperliquid 永续上 Delta 对冲，赚取纯 Vol Edge
13. **清算机器人** — 监控 DeepBook Margin 可清算仓位，自动执行清算赚取奖励
14. **套利 Bot：DeepBook ↔ CEX** — 监控 DeepBook 与 Binance 价差，实时套利
15. **做市 Bot** — 在 Predict 上持续双边报价，赚取买卖价差，自动管理库存风险
16. **预言机延迟套利 Bot** — 利用 Predict 预言机更新延迟，在价格更新前后抢交易
17. **Kelly 仓位管理 Bot** — 根据胜率计算 Kelly 比例，自动调整每笔交易仓位大小
18. **多策略信号 Bot** — 聚合技术指标、链上数据、社交媒体信号，AI 综合评分后自动下单
19. **MEV 保护交易 Bot** — 通过 PTB 将多步操作打包为原子交易，保护免受 MEV 攻击
20. **跨期套利 Bot** — 同一标的不同到期日的 Predict 合约间套利，利用期限结构异常

## Keeper 与基础设施

21. **Settled-Redeem Keeper 网络** — 监控已结算预言机，扫描未赎回仓位，调用 `redeem_permissionless` 赚小费
22. **预言机健康监控** — 实时监控 `OracleSVIUpdated` 事件频率和延迟，异常时告警
23. **提款限制器监控** — 追踪 PLP 提款限制器令牌桶状态，为 LP 提供流动性预警
24. **Keeper 竞价市场** — 多个 Keeper 竞争执行赎回任务，最低小费者获胜，去中心化执行层
25. **Predict 状态索引器** — 高性能索引所有 Predict 事件，提供 REST/WS API 供其他应用使用
26. **Gas 优化 Keeper** — 智能批量处理多个赎回请求到一个 PTB，降低 Gas 成本
27. **自动续期服务** — 监控即将到期的仓位，自动按策略续期或平仓

## 前端与消费者应用

28. **Telegram Quick-Predict Bot** — `/up 70k 15m 100usdc` 命令直接下单，结算后 DM 通知结果
29. **连胜排行榜 PWA** — 每日二元选择，连胜记录，每周奖池，NFT 徽章
30. **游戏化预测 App** — 化身虚拟角色，预测正确获得经验和装备，排行榜和社交分享
31. **社交交易前端** — 关注顶级预测者，一键跟单其仓位，查看其历史胜率
32. **移动优先交易界面** — 针对手机优化的 Predict 交易 UI，滑动手势下单
33. **Predict 桌面组件** — macOS/Windows 桌面小组件，实时显示 BTC 预测价格和你的仓位
34. **Discord 预测 Bot** — 在 Discord 服务器中创建预测房间，成员可下注和查看排行
35. **群聊锦标赛 Bot** — 在微信群/Telegram 群中发起预测锦标赛，自动统计和排名
36. **语音驱动交易** — 语音指令下单："买入 BTC 上涨到 7 万，15 分钟，100 USDC"

## 分析与可视化

37. **Predict Surface Studio** — 实时 3D 波动率曲面可视化（行权价 × 到期日 → IV），带时间旅行回放
38. **PLP 风险仪表盘** — 金库利用率、提款限制器状态、极端场景模拟（±5σ BTC 移动）
39. **交易者分析仪表盘** — 追踪链上交易者地址，统计胜率、盈亏、持仓风格
40. **SVI 曲线对比器** — 并排比较 Predict SVI 与 Polymarket/Hyperliquid 的波动率微笑
41. **结算排行榜** — 按周期统计最准确的预测者，按盈亏排名
42. **实时资金费率面板** — 显示各到期日的隐含利率和资金费率
43. **仓位热力图** — 可视化所有链上仓位的行权价和到期日分布
44. **链上大户追踪** — 追踪大额 Predict 交易，实时推送 Whale Alert

## 开发者工具与 SDK

45. **Predict Python SDK** — Python 封装 Predict 合约调用，支持策略回测和自动化交易
46. **Predict TypeScript SDK** — TypeScript SDK，方便前端集成 Predict 功能
47. **策略回测框架** — 基于历史预言机数据回测 Predict 策略，计算 Sharpe/最大回撤
48. **模拟交易沙盒** — 开发者在沙盒中测试策略，不影响真实仓位
49. **合约事件流处理器** — 实时处理 Predict 合约事件，转换为结构化数据供应用消费
50. **一键部署策略模板** — CLI 工具，从模板快速部署金库策略到测试网
