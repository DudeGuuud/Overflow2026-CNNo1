# Multi-step DeFi Orchestrator / 多步 DeFi 编排器

> "借 USDC → 买 BTC → 质押 → 对冲" — AI 将复杂 DeFi 操作编译为单个 PTB

## 解决什么问题

复杂 DeFi 策略（如杠杆挖矿、跨协议套利、对冲策略）需要用户手动在多个协议间切换操作，每步独立执行存在失败风险和资金安全隐患。如果中间步骤失败，用户可能面临不完整的仓位和意外风险。本项目利用 AI 将复杂 DeFi 操作描述编译为单个 Sui PTB，所有步骤原子化执行——要么全部成功，要么全部回滚，消除部分执行风险。

## 核心功能

- 用户描述复杂 DeFi 策略，AI 解析为多步操作序列并编译为单个 PTB
- 支持跨协议编排：Scallop 借贷、DeepBook 兑换、Navi 质押、Aftermath LP 等
- 依赖分析和并行优化：识别无依赖的操作步骤并行执行，缩短整体执行时间
- 策略模板库：预置常见 DeFi 策略模板（杠杆挖矿、三角套利、delta 中性），用户可一键使用

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| PTB | 将多步 DeFi 操作编译为单个 PTB，原子化执行保证一致性 |
| DeepBook | 编排中的兑换步骤执行 |
| Move 对象模型 | `OrchestratedStrategy` 对象存储策略定义和执行状态 |
| 动态字段 | 为策略附加步骤详情、依赖关系和执行结果 |

## 实现方案

### 智能合约 (Move)
- **`orchestrated_strategy` 模块**：定义 `OrchestratedStrategy` 对象，字段包括 `owner: address`、`strategy_hash: vector<u8>`、`step_count: u8`、`required_protocols: vector<ID>`、`status: u8`、`total_value_locked: u256`
- **`step_validator` 模块**：`validate_step(strategy: &OrchestratedStrategy, step_index: u8, context: ExecutionContext)` 编排时校验每步的前置条件
- **`strategy_executor` 模块**：`execute_strategy(strategy: &mut OrchestratedStrategy, ptb_bytes: vector<u8>)` 执行编译好的 PTB
- 关键数据结构：`StepDef { protocol_id: ID, action: u8, input_coin: Type, output_coin: Type, amount: u64, depends_on: vector<u8> }`

### 前端 / 后端
- **前端**：React + ReactFlow，可视化策略编排 DAG 图、步骤拖拽编辑器、执行预览
- **后端**：Python Agent 服务，策略编译器（将 DAG 编译为 PTB）、协议接口适配层
- 关键功能：策略编辑器、模板市场、执行监控

### AI / Agent 部分
- 策略解析：GPT-4o 将自然语言描述转换为操作 DAG（有向无环图）
- 依赖分析：AI 识别步骤间的数据依赖和时序关系，计算可并行步骤
- 风险评估：独立 LLM 评估策略整体风险（杠杆倍数、协议风险、流动性风险）
- Agent 工作流：解析用户意图 → 构建 DAG → 依赖分析 → 编译 PTB → 风险审查 → 用户确认 → 执行

## 技术架构

用户通过自然语言或可视化编辑器定义多步 DeFi 策略。GPT-4o 解析为操作 DAG，依赖分析器识别可并行步骤并优化执行顺序。PTB 编译器将 DAG 转换为 Sui PTB 字节码，每步映射到对应协议的合约调用。风险评估 LLM 审查策略安全性。用户确认后签名提交，PTB 原子化执行所有步骤——任何一步失败则全部回滚。执行结果通过 OrchestratedStrategy 对象上链，前端展示各步骤执行详情。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐⭐ | 多协议接口适配和原子化执行保证 |
| 前端 | ⭐⭐⭐⭐ | DAG 可视化编辑器和策略编排 UI |
| AI/ML | ⭐⭐⭐⭐ | 策略解析、依赖分析和风险评估 |
| 集成复杂度 | ⭐⭐⭐⭐⭐ | 需要对接所有参与的 DeFi 协议接口 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
