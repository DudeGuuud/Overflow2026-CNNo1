# Collaborative Code Review / 协作式代码审查

> 安全 Agent、性能 Agent、风格 Agent 各自审查代码，结果汇总在 Walrus / Security Agent, Performance Agent, and Style Agent each review code with results aggregated in Walrus

## 解决什么问题

代码审查需要从安全、性能、可维护性等多个维度进行，但人工审查难以全面覆盖，单一 AI 也容易忽略某些方面。通过多个专业 Agent 分别从不同角度审查代码，结果汇总在 Walrus，确保审查的全面性和结果的可追溯性。

## 核心功能

- 安全 Agent 审查：检测漏洞、权限问题、输入验证缺陷
- 性能 Agent 审查：分析 Gas 消耗、循环复杂度、存储模式优化
- 风格 Agent 审查：检查代码规范、命名一致性、注释完整性
- 综合审查报告：汇总各 Agent 发现，按严重程度排序并给出修复建议

## 使用的 Walrus / Sui 能力

| 能力 | 用途 |
|------|------|
| Walrus Blob 存储 | 存储各 Agent 审查结果、综合报告、代码上下文快照 |
| MemWal 记忆层 | 记住项目特有的代码规范和历史问题模式 |
| Sui 对象模型 | 链上记录审查请求、各 Agent 状态、综合评分 |
| Walrus 读取 API | 读取历史审查结果进行趋势分析 |

## 实现方案

### Agent / AI 部分
- 使用 CrewAI 构建三角色审查团队
- 安全 Agent：GPT-4o + Slither 静态分析 + 自定义规则
- 性能 Agent：Claude 3.5 Sonnet，擅长 Gas 优化和算法分析
- 风格 Agent：GPT-4o + ESLint/Solhint 规则映射
- 协调 Agent 使用 LangGraph 汇总各 Agent 结果

### Walrus 集成
- 安全报告以 `reviews/{review_id}/security.json` 存储
- 性能报告以 `reviews/{review_id}/performance.json` 存储
- 风格报告以 `reviews/{review_id}/style.json` 存储
- 综合报告以 `reviews/{review_id}/summary.md` 存储
- Sui 链上 ReviewRequest 对象管理审查生命周期

### 前端 / 后端
- 前端：React + Monaco Editor，代码展示和审查标注
- 后端：Python FastAPI，协调多 Agent 并行执行
- 关键页面：提交审查页、审查进度页（三 Agent 状态）、综合报告页、历史趋势页

## 技术架构

用户提交代码（GitHub PR 或直接粘贴）→ 代码快照写入 Walrus → 并行触发三个审查 Agent → 各 Agent 独立分析并将结果写入 Walrus → 协调 Agent 汇总生成综合报告 → 综合报告写入 Walrus → Sui 链上更新 ReviewRequest 状态 → 前端展示各维度结果和综合报告。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Walrus 集成 | ⭐⭐ | 并行写入 + 综合报告聚合 |
| AI/Agent | ⭐⭐⭐ | 代码理解和多维度分析质量 |
| 前端 | ⭐⭐⭐ | 代码展示和审查标注交互 |
| 集成复杂度 | ⭐⭐⭐ | 多 Agent 并行 + GitHub 集成 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
