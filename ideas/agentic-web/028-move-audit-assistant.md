# Move Contract Audit Assistant / Move 合约 AI 审计助手

> 输入 Move 代码，AI 逐函数分析安全性，标注风险等级和改进建议

## 解决什么问题

Move 合约安全审计需要高度专业的知识和大量人工审查时间，专业审计公司费用高昂，中小项目难以负担。现有自动化工具仅做静态模式匹配，无法理解业务逻辑层面的安全问题。本项目通过 AI 深度分析 Move 合约代码，逐函数标注风险等级，生成结构化审计报告和修复建议，降低 Sui 生态合约安全门槛。

## 核心功能

- 上传 Move 包后 AI 自动解析模块结构，逐函数分析安全性并标注风险等级（Critical/High/Medium/Low）
- 识别 Sui 特有风险模式：对象所有权泄漏、能力误用、PTB 组合漏洞、共享对象竞争条件
- 生成结构化审计报告，包含漏洞描述、代码位置、影响分析和修复代码建议

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `AuditReport` 对象存储审计结果，包含风险数量、严重度分布和哈希摘要 |
| 动态字段 | 为每个发现项附加详细信息（代码位置、严重度、修复建议） |
| 事件（Event） | 发出 `AuditCompletedEvent` 通知项目方和社区审计完成 |
| 链上验证 | 审计报告哈希上链确保结果不可篡改，支持审计历史溯源 |

## 实现方案

### 智能合约 (Move)
- **`audit_registry` 模块**：定义 `AuditReport` 对象，字段包括 `package_id: ID`、`source_hash: vector<u8>`、`critical_count: u64`、`high_count: u64`、`medium_count: u64`、`low_count: u64`、`auditor: address`、`timestamp: u64`
- **`finding` 模块**：`add_finding(report: &mut AuditReport, severity: u8, title: String, location: String)` 记录漏洞发现
- **`report_access` 模块**：`verify_report(report: &AuditReport, source_hash: vector<u8>): bool` 验证报告与源码对应
- 关键数据结构：`Finding { severity: u8, title: String, location: String, description: String, suggestion: String }`

### 前端 / 后端
- **前端**：React + Monaco Editor，代码查看器、风险高亮、侧边栏审计报告、修复建议对比
- **后端**：Python Agent 服务，代码解析引擎、AI 分析管道、报告生成器
- 关键功能：代码上传、实时审计进度、报告导出（PDF/Markdown）、历史审计记录

### AI / Agent 部分
- Move 代码解析：自定义 AST 解析器提取函数签名、能力约束、对象交互模式
- 安全分析：GPT-4o 结合 RAG 检索已知 Move 漏洞库，逐函数评估安全性
- 模式检测：识别 Sui 特有反模式（无限循环铸造、对象吞噬、权限提升）
- Agent 工作流：代码上传 → AST 解析 → 逐函数分析 → 模式匹配 → 风险评分 → 报告生成 → 链上存证

## 技术架构

用户上传 Move 包源码，后端 AST 解析器提取模块结构和函数签名。Agent 将每个函数的代码上下文输入 GPT-4o，结合 RAG 检索的 Move 漏洞知识库进行安全评估。模式检测引擎并行扫描 Sui 特有反模式。分析结果汇总为结构化报告，按严重度排序并附修复代码建议。报告摘要哈希写入 `AuditReport` Move 对象上链存证。前端展示交互式代码审计视图。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 审计报告存储和验证逻辑 |
| 前端 | ⭐⭐⭐ | 代码编辑器集成和审计结果可视化 |
| AI/ML | ⭐⭐⭐⭐ | Move 代码理解、漏洞模式识别和修复建议生成 |
| 集成复杂度 | ⭐⭐⭐ | AST 解析和知识库构建 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
