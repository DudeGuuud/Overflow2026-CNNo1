# Sui Overflow 2026 - CN No.1

Sui Overflow 2026 中文区项目登记仓库。用于统计中文区参赛项目、抽奖和发放周边。

> **注意：本仓库仅用于中文区统计和社区活动（抽奖/周边），正式参赛请以 [Sui Overflow 官网](https://overflow.sui.io/) 提交为准。**
> **官方手册：[Overflow 2026 Handbook](https://mystenlabs.notion.site/overflow-2026-handbook)**

## Quick Links / 快速入口

- [赛道介绍 / Tracks](tracks/)
- [项目灵感 / Ideas](ideas/) (50 ideas/track)
- [开发者提交指南 / Guide](guide.md)
- [常见问题 / FAQ](faq.md)
- [项目提交模板 / Submission Template](submissions/_template/README.md)

## How to Submit / 如何提交

**3 步完成登记 / 3 steps to register:**

1. **Fork** 本仓库 / Fork this repo
2. 复制 `submissions/_template/` 目录，重命名为你的 **GitHub ID** / Copy the template folder and rename to your GitHub ID
3. 编辑 `README.md` 填写项目信息，提交 **Pull Request** / Edit the README and submit a PR

```
submissions/
├── _template/           # <-- 复制这个目录
│   └── README.md
├── your-github-id/      # <-- 粘贴并重命名
│   ├── README.md        # 第一个项目
│   ├── project1.md      # 第二个项目（可选）
│   └── project2.md      # 第三个项目（可选）
```

## Tracks / 赛道

| Track | 赛道 | 首奖 | 详情 |
|-------|------|------|------|
| Agentic Web | AI 原生智能体 | $30,000 | [查看](tracks/agentic-web.md) |
| DeFi & Payments | 去中心化金融与支付 | $30,000 | [查看](tracks/defi-payments.md) |
| Walrus | Walrus 可验证数据 | $35,000 | [查看](tracks/walrus.md) |
| DeepBook | DeepBook 链上预测市场 | $35,000 | [查看](tracks/deepbook.md) |

## Timeline / 时间线

| 阶段 | 时间 (Pacific Time) | 说明 |
|------|------|------|
| 黑客松启动 | Sui Live Miami 期间 | 赛道公布、奖池公布、黑客松正式开启 |
| 开发阶段 | 启动后 ~2 个月 | 活跃构建期，参与 Workshop 获取资源和支持 |
| Workshop 系列 | 开发期间 | Overflow 团队覆盖参赛所需资源和构建指导 |
| 提交截止 | 待公布 | 所有项目提交的最终截止日期 |
| 入围公布 | 截止后 | 入围团队公布，入围团队将在 Demo Day 现场展示 |
| Demo Day | 入围后 | 入围团队现场展示项目 |
| 最终获奖 | Demo Day 后 | 获奖者公布，受邀在 Sui Basecamp 2026 上 Pitch |

## Prizes / 奖金

**奖金分配规则：50/50 分期发放**

- 获奖公布时发放 **50%**
- 成功部署主网后发放剩余 **50%**
- 截止时已部署主网的项目 → **100% 一次性发放**

**特别奖：**

| Award | Prize | 条件 |
|-------|-------|------|
| University / 大学团队奖 | $2,500 x 10 | 团队至少 50% 成员为在校学生 |

## 官方提交要求

正式参赛需在官网提交以下材料：

| 字段 | 要求 |
|------|------|
| 项目名称 | 简洁明了 |
| 项目描述 | 做什么，为什么重要 |
| 项目 Logo | 1:1 比例 (JPG/PNG) |
| GitHub 仓库 | 评审期间必须公开 |
| Demo 视频 | 必须，YouTube 优先，≤ 5 分钟 |
| 网站 | 可选，强烈建议 |
| 部署环境 | Testnet 或 Mainnet |
| Package ID | 如已部署链上 |

## Judging Criteria / 评审标准

| 维度 | 说明 |
|------|------|
| 产品体验 | 质量、可用性、打磨程度和整体用户体验 |
| 解决问题 | 有意义的问题、市场相关性和长期价值 |
| 技术质量 | 可靠性、与 Sui 的深度整合 |
| 表达与愿景 | 表达清晰、叙事和长期愿景 |

> Overflow 注重有意义的产品和生态影响，而非纯技术演示。

## Resource Hub / 资源中心

| 资源 | 说明 |
|------|------|
| [Seal](https://docs.sui.io) | Walrus 和 MemWal 的隐私层 |
| [Walrus](https://walrus.site) | AI 和链上金融的可验证数据平台 |
| [DeepBook](https://deepbook.tech) | Sui 链上流动性层（现货、保证金、预测市场） |
| [OtterSec](https://osec.io) | 链上安全审计标准 |
| [Scallop](https://scallop.io) | Sui 生态下一代货币市场 |
| [Handbook](https://mystenlabs.notion.site/overflow-2026-handbook) | 官方参赛手册 |

**中文学习资源：**
- [Learning Sui](https://hoh-zone.github.io/learning-sui/) — Sui 中文教程（覆盖 Move 语言、对象模型、PTB 等基础，适用 Agentic Web / DeFi & Payments）
- [DeepBook Bootcamp](https://github.com/hoh-zone/deepbook-bootcamp) — DeepBook 中文实战教程（适用 DeepBook 赛道）
- [Let's Walrus](https://github.com/hoh-zone/lets-walrus) — Walrus 中文入门教程（适用 Walrus 赛道）

**中文参考文档：**
- [Walrus 中文参考文档](https://hoh-zone.github.io/intro-walrus) — Walrus 概念与入门
- [DeepBook 中文参考文档](https://hoh-zone.github.io/intro-deepbook) — DeepBook 概念与入门
- [DeFi 中文参考文档](https://hoh-zone.github.io/intro-defi) — DeFi 概念与入门

**赛道 Problem Statements：**
- [Agentic Web](https://mystenlabs.notion.site/agentic-web-problem-statement)
- [DeFi & Payments](https://mystenlabs.notion.site/defi-payments-problem-statement)
- [Walrus](https://mystenlabs.notion.site/walrus-track-problem-statement)
- [DeepBook](https://mystenlabs.notion.site/deepbook-predict-problem-statement)

## Important / 重要说明

- 本仓库用于**中文区项目登记**，方便统计参赛情况和发放周边
- **正式参赛**仍需在 [Sui Overflow 官网](https://overflow.sui.io/) 完成提交
- 在本仓库登记后自动获得中文区**抽奖资格**
- 每个 GitHub ID 只能创建一个目录，项目必须基于 Sui

## CN Raffle / 中文区抽奖

**奖品：Sui Play 游戏机 x1**

参与规则：
- PR 合并 **+ 项目满足官方参赛要求** → 获得 **1 次抽奖机会** + **1 份周边**
- 抽奖时间另行通知

**注意事项：**
- 周边和游戏机仅限**大陆收货地址**，不支持折现

---

Built with love by the Sui CN community
