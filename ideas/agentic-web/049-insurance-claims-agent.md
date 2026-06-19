# Insurance Claims Agent / 保险理赔自动 Agent

> AI 评估理赔条件（链上数据 + 外部数据），满足条件自动通过 PTB 执行赔付

## 解决什么问题

传统保险理赔流程漫长，需要大量人工审核和文件提交，用户体验差且成本高。去中心化保险协议的参数化赔付虽已有先例，但复杂理赔场景（如部分损失、多因子触发）仍需人工判断。本项目通过 AI Agent 综合评估链上数据（链上资产状态、协议事件）和外部数据（预言机价格、天气数据），智能判断理赔条件是否满足，满足时自动通过 PTB 执行赔付，实现快速透明的保险理赔。

## 核心功能

- 多数据源理赔评估：AI 综合分析链上资产状态、协议事件和外部数据判断理赔触发条件
- 自动化赔付流程：条件满足时构造 PTB 从理赔池自动赔付到受益人地址
- 争议处理：AI 低置信度判断转社区投票裁决，支持申诉和补充证据

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `InsurancePolicy`、`ClaimRecord`、`ClaimPool` 对象管理保单、理赔和资金池 |
| PTB | 将「验证理赔条件 → 计算赔付金额 → 从池中提取 → 转账给受益人」原子化执行 |
| 共享对象 | `ClaimPool` 共享对象管理保险资金池，支持多人同时理赔 |
| 事件（Event） | 发出 `ClaimSubmittedEvent`、`ClaimApprovedEvent`、`ClaimDisputedEvent` 通知理赔流程 |

## 实现方案

### 智能合约 (Move)
- **`insurance_policy` 模块**：定义 `InsurancePolicy` 对象，字段包括 `policy_id: ID`、`insured: address`、`coverage_type: u8`、`coverage_amount: u64`、`premium_paid: u64`、`start_epoch: u64`、`end_epoch: u64`、`status: u8`、`claim_count: u64`
- **`claim_record` 模块**：`submit_claim(policy: &InsurancePolicy, evidence: vector<u8>, claim_amount: u64): ClaimRecord` 提交理赔
- **`claim_pool` 模块**：`process_payout(pool: &mut ClaimPool, claim: &ClaimRecord, amount: u64)` 执行赔付
- 关键数据结构：`ClaimRecord { claim_id: ID, policy_id: ID, claimant: address, evidence_hash: vector<u8>, ai_verdict: u8, payout_amount: u64, status: u8, timestamp: u64 }`

### 前端 / 后端
- **前端**：React + TailwindCSS，保单管理、理赔提交面板、理赔进度追踪、资金池状态
- **后端**：Python Agent 服务，理赔评估引擎、多源数据采集器、赔付计算器
- 关键功能：购买保单、提交理赔、进度查询、赔付记录、资金池监控

### AI / Agent 部分
- 理赔评估：GPT-4o 综合分析保单条款、链上事件证据和外部数据，判断是否满足理赔条件
- 赔付计算：根据损失评估模型计算合理赔付金额，考虑免赔额和赔付比例
- 欺诈检测：异常检测模型识别可疑理赔模式（如频繁小额理赔、关联地址串谋）
- Agent 工作流：理赔提交 → 证据采集 → AI 评估 → 赔付计算 → 置信度判断 → 自动赔付/人工审核

## 技术架构

用户购买保险后获得 `InsurancePolicy` Move 对象。理赔时提交证据材料，Agent 自动采集相关链上数据（如 DeFi 协议清算事件、预言机价格突变）和外部数据（天气 API、航班数据）。GPT-4o 综合评估证据与保单条款的匹配度，计算赔付金额。高置信度通过时自动构造 PTB 从 `ClaimPool` 执行赔付。低置信度或有争议的理赔转社区投票。前端提供保单管理和理赔追踪。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐⭐ | 保单管理、理赔状态机和资金池安全 |
| 前端 | ⭐⭐⭐ | 保单面板、理赔流程和资金池监控 |
| AI/ML | ⭐⭐⭐⭐ | 多源理赔评估、赔付计算和欺诈检测 |
| 集成复杂度 | ⭐⭐⭐⭐ | 多数据源集成、理赔流程编排和资金安全 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
