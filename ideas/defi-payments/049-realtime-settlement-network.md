# 实时结算支付网络 / Real-Time Settlement Network

> 面向商户的即时结算方案，交易确认即到账，无 T+2 延迟

## 解决什么问题

传统支付系统中商户收到的款项通常需要 T+1 到 T+2 个工作日才能结算到账，严重影响中小商户的现金流和运营效率。跨境支付的结算周期更长，还需承担高昂的手续费和汇率损失。实时结算支付网络基于 Sui 区块链的亚秒级确认特性，让商户在交易完成后立即收到资金，同时支持多币种自动兑换和统一结算，消除传统支付的时间延迟和高额成本。

## 核心功能

- 即时交易结算：消费者支付后资金秒级到达商户地址，无等待期
- 多币种自动路由：消费者可用任意代币支付，系统自动兑换为商户指定币种
- 统一结算面板：商户查看实时收入、交易明细和结算状态
- 资金自动分流：商户可配置收入按比例自动分配到运营、税务和储备账户
- 结算凭证生成：每笔交易生成链上可验证的结算凭证，便于对账和审计

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| PTB | 原子化执行"消费者支付 → DEX 兑换 → 商户到账 → 手续费扣除" |
| Coin 标准 | 统一处理消费者支付代币和商户结算代币 |
| 共享对象 | PaymentRouter 作为共享对象处理并发交易 |
| 对象所有权 | MerchantAccount 对象绑定商户地址和配置 |
| 动态字段 | 存储交易记录、手续费率和结算偏好 |

## 实现方案

### 智能合约 (Move)
- `merchant_registry` 模块：商户注册、KYC 绑定和结算配置
- `payment_router` 模块：路由消费者支付到正确的兑换和结算流程
- `auto_swap` 模块：通过 DEX（DeepBook/Cetus）自动兑换消费者代币为商户目标币种
- `settlement_ledger` 模块：记录所有交易和结算状态，支持对账查询
- 关键数据结构：`MerchantAccount { owner: address, accepted_coins: vector<Type>, settlement_coin: Type, fee_rate: u64, daily_volume: u64, total_settled: Coin<T> }`

### 前端 / 后端
- 技术栈：Next.js + @mysten/dapp-kit + Sui TypeScript SDK + WebSocket
- 关键页面：Merchant Dashboard（商户后台）、Transaction Feed（实时交易流）、Settlement Report（结算报告）、Payment Integration（支付集成指南）、Consumer Checkout（消费者支付页）
- 后端提供 WebSocket 实时推送、交易分析和 webhook 通知
- 支付 SDK 提供 React 组件和 REST API 两种接入方式，商户可灵活选择

## 技术架构

商户在前端注册并配置结算偏好（目标币种、手续费率、结算地址）。消费者发起支付时，PaymentRouter 构建动态 PTB：从消费者地址扣除支付代币 → 通过 DEX 兑换为商户目标币种（如需）→ 将手续费转入协议金库 → 将净额转入商户地址。整个流程在 Sui 的亚秒级确认中完成，商户即时到账。前端通过 WebSocket 实时推送新交易通知，商户可查看交易流和每日结算报告。支付集成通过 SDK 提供，商户网站只需引入一行代码即可接入。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 支付路由和自动兑换逻辑清晰可实现 |
| 前端 | ⭐⭐⭐⭐ | 商户后台和实时交易展示需要精细设计 |
| 集成复杂度 | ⭐⭐⭐⭐ | 需要对接 DEX 实现可靠兑换和低延迟推送 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
