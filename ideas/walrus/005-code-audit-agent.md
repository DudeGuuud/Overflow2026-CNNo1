# Code Audit Agent / 代码审计智能体

> 持续监控 GitHub 仓库变更，审计结果存储在 Walrus，形成完整安全审计历史 / Continuously monitors GitHub repos, storing audit results in Walrus for complete security audit history

## 解决什么问题

智能合约安全审计通常是离散事件，缺乏持续的变更追踪。每次代码修改都可能引入新漏洞，但现有工具难以自动关联历史审计结果。将每次审计结果存储在 Walrus 可形成不可篡改的审计历史，便于追溯和对比。

## 核心功能

- GitHub Webhook 监控：自动检测仓库的 Push/PR 事件，触发审计流程
- 多维度安全审计：检查重入攻击、溢出漏洞、权限控制、Gas 优化等问题
- 审计历史追溯：所有审计结果存储在 Walrus，可对比任意两个版本的差异
- 风险趋势可视化：展示代码库安全风险随时间的变化趋势

## 使用的 Walrus / Sui 能力

| 能力 | 用途 |
|------|------|
| Walrus Blob 存储 | 存储审计报告（JSON）、代码快照差异（Diff）、风险评分 |
| MemWal 记忆层 | 记住已知的漏洞模式和项目特有的安全策略 |
| Sui 对象模型 | 链上记录仓库注册信息、审计历史索引、风险评级 |
| Walrus 读取 API | 按版本号获取历史审计报告进行对比 |

## 实现方案

### Agent / AI 部分
- 使用 AutoGen 构建多 Agent 审计团队：静态分析 Agent、AI 审查 Agent、报告 Agent
- 静态分析集成 Slither、Mythril 工具
- LLM 使用 Claude 3.5 Sonnet 进行深度代码逻辑审查
- 风险评分使用 CVSS 标准量化

### Walrus 集成
- 审计报告以 `audits/{repo}/{commit_sha}.json` 存储
- 代码差异快照以 `diffs/{repo}/{commit_sha}.patch` 存储
- 每月汇总报告以 `summaries/{repo}/{month}.md` 存储
- Sui 链上 AuditRecord 对象关联 commit SHA 和 Blob ID

### 前端 / 后端
- 前端：Vue 3 + Element Plus，展示审计结果和风险趋势
- 后端：Python FastAPI + GitHub Webhook 处理
- 关键页面：仓库管理页、审计详情页、版本对比页、风险趋势页

## 技术架构

GitHub Webhook 触发 → 拉取代码差异 → 静态分析工具扫描 → AI Agent 深度审查 → 生成审计报告 → 报告写入 Walrus Blob → Sui 链上创建 AuditRecord → 前端查询链上记录 → 从 Walrus 获取报告渲染。用户可通过 MemWal 让 Agent 记住项目特有规则。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Walrus 集成 | ⭐⭐ | 按版本号的 Blob 存储 |
| AI/Agent | ⭐⭐⭐⭐ | 代码安全审计需要深度理解 |
| 前端 | ⭐⭐⭐ | 版本对比和差异高亮展示 |
| 集成复杂度 | ⭐⭐⭐⭐ | Webhook + 多工具集成 + 报告生成 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
