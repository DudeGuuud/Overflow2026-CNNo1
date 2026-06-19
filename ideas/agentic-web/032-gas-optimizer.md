# Gas Optimizer / Gas 分析优化器

> 分析 PTB 执行路径，AI 推荐 Gas 优化方案，预估节省百分比

## 解决什么问题

Sui 上的复杂 PTB 交易 Gas 消耗可能很高，但开发者缺乏直观的工具来分析和优化 Gas 使用。不合理的对象引用方式、冗余的拆币/合并操作、低效的函数调用顺序都会浪费 Gas。本项目通过 AI 分析 PTB 执行路径和链上 Gas 消耗模式，推荐具体的优化方案并预估节省比例，帮助开发者和用户降低链上操作成本。

## 核心功能

- 分析已提交 PTB 的 Gas 消耗明细，按指令粒度拆解并可视化各步骤 Gas 占比
- AI 识别 Gas 优化机会：对象引用优化、指令重排序、冗余操作消除、批量合并建议
- 模拟优化后的 PTB 执行，对比展示优化前后的 Gas 消耗和预估节省

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| PTB | 分析目标，解析 PTB 指令序列并重构优化后的 PTB |
| Move 对象模型 | `GasReport` 对象存储 Gas 分析结果和优化建议，可分享给团队 |
| 链上数据 | 读取历史交易的 Gas 消耗数据，建立 Gas 预估模型 |
| Dry Run | 模拟优化后 PTB 的实际 Gas 消耗，验证优化效果 |

## 实现方案

### 智能合约 (Move)
- **`gas_report` 模块**：定义 `GasReport` 对象，字段包括 `digest: vector<u8>`、`total_gas: u64`、`optimized_gas: u64`、`saving_pct: u64`、`optimizer: address`、`timestamp: u64`
- **`optimization_log` 模块**：`log_optimization(report: &mut GasReport, category: u8, saved: u64, description: String)` 记录每项优化
- **`benchmark` 模块**：`record_benchmark(module: String, function: String, gas_used: u64, input_size: u64)` 建立合约 Gas 基准
- 关键数据结构：`GasBreakdown { command: String, gas_used: u64, gas_pct: u64, category: u8, suggestion: String }`

### 前端 / 后端
- **前端**：React + D3.js，Gas 消耗饼图、指令级火焰图、优化前后对比面板
- **后端**：TypeScript 服务，PTB 解析器、Gas 分析引擎、优化建议生成
- 关键功能：交易 Digest 输入、Gas 明细可视化、一键优化、对比报告导出

### AI / Agent 部分
- 模式识别：GPT-4o 分析 PTB 指令序列，识别已知的 Gas 浪费模式（重复拆币、不必要的对象获取）
- 优化推荐：基于 Gas 消耗热力图，AI 排序优化机会并生成具体代码修改建议
- 预估模型：历史 Gas 数据训练回归模型，预测优化后的 Gas 消耗
- Agent 工作流：交易解析 → 指令级 Gas 拆解 → 模式匹配 → 优化建议生成 → Dry Run 验证 → 报告输出

## 技术架构

用户输入交易 Digest 或粘贴 PTB 代码，解析引擎提取指令序列和实际 Gas 消耗。Gas 分析引擎按指令粒度计算 Gas 占比，生成消耗热力图。GPT-4o 识别 Gas 浪费模式并排序优化机会。优化器生成修改后的 PTB 代码，通过 Dry Run API 验证实际 Gas 节省。分析结果写入 `GasReport` Move 对象上链存证。前端以火焰图和对比面板直观展示优化效果。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | Gas 报告存储和基准记录 |
| 前端 | ⭐⭐⭐ | Gas 消耗可视化和对比面板 |
| AI/ML | ⭐⭐⭐ | Gas 模式识别和优化建议生成 |
| 集成复杂度 | ⭐⭐⭐ | PTB 解析和 Dry Run API 对接 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
