# 支付 SDK / Payment SDK

> 一行代码集成 Sui 支付到任何网站或 App，支持多币种和自动兑换

## 解决什么问题

Web2 开发者想在网站或 App 中集成加密货币支付，但现有的 Sui 支付集成需要深入了解区块链开发、钱包连接和交易构建。这导致大量潜在商家被挡在门外。支付 SDK 提供一行代码的集成方案，开发者无需了解区块链即可在自己的产品中接入 Sui 支付，支持多币种自动兑换和即时结算。

## 核心功能

- 一行代码集成：`suiPay({ amount: 10, currency: 'USDC' })` 即可渲染支付按钮
- 自动钱包连接：内置钱包连接逻辑，支持所有主流 Sui 钱包
- 多币种自动兑换：用户可选择任意代币支付，SDK 自动处理兑换
- 支付状态回调：支付完成后回调开发者指定的 webhook，更新业务状态

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| PTB | 构建支付交易：扣款 → 兑换 → 结算 → 回调 |
| Coin 标准 | 统一处理多种支付代币 |
| 对象所有权 | MerchantConfig 对象管理商户配置 |
| 事件 (Events) | 发出支付事件触发 webhook 回调 |
| DeepBook 集成 | SDK 内部调用 DEX 实现代币兑换 |

## 实现方案

### 智能合约 (Move)
- `sdk_payment` 模块：处理 SDK 发起的支付请求
- `merchant_config` 模块：存储商户配置（结算地址、支持的币种、手续费）
- `swap_router` 模块：通过 DeepBook 自动兑换用户代币为商户目标代币
- `callback` 模块：发出支付事件，包含订单 ID 和支付详情
- 关键数据结构：`PaymentRequest { merchant: address, order_id: String, amount: u64, token: Type, status: u8, created_at: u64 }`

### 前端 / 后端
- 技术栈：TypeScript SDK (NPM Package) + React Component + Webhook Server
- 关键组件：`<SuiPayButton>`（支付按钮组件）、`useSuiPayment`（React Hook）、`SuiPayWidget`（支付弹窗）
- 后端接收 webhook 回调，验证支付并更新业务订单状态

## 技术架构

开发者通过 NPM 安装 SDK，在页面中添加 `<SuiPayButton amount={10} currency="USDC" orderId="order_123" onSuccess={handleSuccess} />`。用户点击按钮后，SDK 弹出支付窗口，自动连接钱包，构建 PTB：从用户钱包扣除代币 → 通过 DeepBook 兑换为商户目标代币 → 扣除手续费 → 结算到商户地址。支付成功后 SDK 回调 onSuccess 并通过 webhook 通知商户后端。整个过程开发者无需编写任何区块链代码。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 支付处理和兑换逻辑较直接 |
| 前端 | ⭐⭐⭐⭐ | SDK 需要优秀的开发者体验和文档 |
| 集成复杂度 | ⭐⭐⭐ | 需要对接 DEX 和提供多平台 SDK |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
