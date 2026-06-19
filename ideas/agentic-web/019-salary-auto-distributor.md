# Salary Auto-Distributor / 薪资自动分配器

> 收到工资后，AI 按用户描述的规则自动分配到储蓄、投资、支付等子钱包

## 解决什么问题

链上收入（ DAO 赏金、质押收益、协议收入）缺乏自动化的资金分配管理工具。用户收到资金后需要手动将资金分配到不同用途的账户，容易遗忘或随意支配，无法系统性地执行财务规划。本项目利用 AI 解析用户的分配规则（自然语言描述），收到资金后自动通过 PTB 将资金按比例分配到预设的子钱包地址，实现链上薪资管理的自动化。

## 核心功能

- 自然语言设定分配规则："工资的 50% 存 Scallop 储蓄，30% 买 SUI 质押，20% 留作日常"
- 监控指定地址的收入事件，触发自动分配
- 分配规则通过 Move 策略对象链上存储，保证规则不可被篡改
- 分配历史和资金流向可视化追踪

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| PTB | 原子化执行「拆分资金 → 按比例分配 → 兑换 → 存入协议 → 转账到子钱包」 |
| Move 对象模型 | `DistributionRule` 对象存储分配比例和目标地址 |
| 事件（Event） | 发出 `IncomeReceivedEvent` 触发分配和 `DistributionCompletedEvent` 记录结果 |
| Coin 管理 | 收入 Coin 的精确拆分和定向转账 |

## 实现方案

### 智能合约 (Move)
- **`distribution_rule` 模块**：定义 `DistributionRule` 对象，字段包括 `owner: address`、`source_address: address`、`allocations: vector<Allocation>`、`min_trigger_amount: u64`、`active: bool`、`total_distributed: u256`
- **`allocation` 模块**：`Allocation` 结构包含 `target_address: address`、`percentage_bp: u64`、`action: u8`（transfer/deposit/swap_deposit）、`protocol_id: Option<ID>`
- **`distributor` 模块**：`distribute(rule: &mut DistributionRule, income: Coin<SUI>)` 按规则拆分并分配资金
- 关键数据结构：`DistributionRecord { rule_id: ID, total_amount: u64, allocations: vector<AllocationResult>, timestamp: u64 }`

### 前端 / 后端
- **前端**：React + TailwindCSS，规则创建向导、资金流向图、分配历史时间线
- **后端**：TypeScript Agent 服务，监听收入事件、解析规则、构造分配 PTB
- 关键功能：规则编辑器、收入监控、分配统计

### AI / Agent 部分
- 规则解析：GPT-4o 从自然语言中提取分配比例和目标操作
- 智能建议：AI 根据用户历史支出模式和收入规模建议优化分配比例
- 异常检测：LLM 审查每次分配是否合理，防止恶意规则导致资金流失
- Agent 工作流：监控收入事件 → 读取分配规则 → 计算各目标金额 → 构造分配 PTB → 提交执行

## 技术架构

用户通过前端用自然语言描述分配规则，GPT-4o 解析为结构化 Allocation 列表。确认后创建 DistributionRule 链上对象。Agent 服务通过 Sui RPC 订阅源地址的 Coin 接收事件，收到资金后读取对应 DistributionRule，计算各目标的分配金额，构造 PTB（拆分 Coin → 按规则执行 transfer/swap/deposit），原子化提交。分配完成后发出 DistributionCompletedEvent，前端实时更新资金流向可视化。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 分配规则管理和多目标资金拆分 |
| 前端 | ⭐⭐⭐ | 资金流向可视化和规则编辑 |
| AI/ML | ⭐⭐ | 自然语言规则解析和异常检测 |
| 集成复杂度 | ⭐⭐⭐ | 收入事件监控和多协议存入操作 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
