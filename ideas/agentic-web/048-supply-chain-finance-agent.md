# Supply Chain Finance Agent / 供应链金融 Agent

> AI 评估供应链风险，自动触发链上付款、开立信用证，通过 PTB 原子化结算

## 解决什么问题

传统供应链金融流程繁琐，依赖大量纸质文件和人工审批，付款周期长、成本高。中小企业因信用记录不足难以获得融资，供应链风险难以实时评估。本项目利用 AI 实时评估供应链各环节的风险状况，根据链上数据（物流确认、质检结果、合同状态）自动触发付款和信用证开立，通过 PTB 原子化结算，大幅提升供应链金融的效率和透明度。

## 核心功能

- AI 实时评估供应链各参与方的信用风险和履约能力，动态调整授信额度
- 基于链上里程碑（发货确认、质检通过、签收确认）自动触发付款 PTB
- 信用证全流程链上管理：开立、通知、承兑、付款全程可追溯

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `LetterOfCredit`、`ShipmentOrder`、`PaymentEscrow` 对象管理供应链金融全流程 |
| PTB | 将「验证里程碑 → 释放付款 → 更新信用额度 → 发出通知」编排为原子交易 |
| 共享对象 | `SupplyChainContract` 共享对象允许多方（买方、卖方、物流、银行）协作更新状态 |
| 事件（Event） | 发出 `ShipmentConfirmedEvent`、`PaymentReleasedEvent` 等通知供应链状态变更 |

## 实现方案

### 智能合约 (Move)
- **`letter_of_credit` 模块**：定义 `LetterOfCredit` 对象，字段包括 `lc_id: ID`、`buyer: address`、`seller: address`、`amount: Balance<SUI>`、`status: u8`（Draft/Issued/Confirmed/Drawn/Settled）、`expiry_epoch: u64`、`conditions: vector<u8>`
- **`shipment_order` 模块**：`create_shipment(lc: &LetterOfCredit, logistics: address, goods_hash: vector<u8>): ShipmentOrder` 创建货运订单
- **`payment_escrow` 模块**：`release_payment(shipment: &ShipmentOrder, lc: &mut LetterOfCredit, proof: vector<u8>)` 条件满足后释放付款
- 关键数据结构：`Milestone { milestone_type: u8, status: u8, verifier: address, evidence_hash: vector<u8>, timestamp: u64 }`

### 前端 / 后端
- **前端**：React + Ant Design Pro，供应链看板、信用证管理、货运追踪、付款面板
- **后端**：Python Agent 服务，风险评估引擎、里程碑监控器、文档验证器
- 关键功能：信用证开立、货运追踪、自动付款、风险评估、供应链可视化

### AI / Agent 部分
- 风险评估：AI 分析参与方历史履约数据、行业风险和市场状况，生成信用风险评分
- 里程碑验证：AI 验证供应链事件（物流确认、质检报告、签收凭证）的真实性
- 欺诈检测：异常检测模型识别可疑的供应链行为（如虚假发货、重复索赔）
- Agent 工作流：合同创建 → 里程碑定义 → 事件监控 → AI 验证 → 条件判定 → PTB 执行付款

## 技术架构

买方通过 PTB 创建 `LetterOfCredit` 对象并存入资金至托管合约。卖方确认后创建 `ShipmentOrder`，物流方更新货运里程碑。AI Agent 持续监控各里程碑事件，验证提交的证据数据。当所有付款条件满足后，Agent 自动构造 PTB 从 `PaymentEscrow` 释放资金给卖方。风险评估引擎定期评估供应链健康度，动态调整授信额度。前端展示供应链全景视图。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐⭐ | 信用证、托管和多方协作状态机 |
| 前端 | ⭐⭐⭐ | 供应链看板和多方协作面板 |
| AI/ML | ⭐⭐⭐ | 风险评估、证据验证和欺诈检测 |
| 集成复杂度 | ⭐⭐⭐⭐ | 多方协作、文档验证和供应链流程编排 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
