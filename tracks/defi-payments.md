# DeFi & Payments

**去中心化金融与支付**

## Problem Statement / 核心命题

Payments and DeFi today are disconnected: payments are static transfers, DeFi is complex and siloed, users must manually orchestrate everything. On Sui, this changes — payments can become programmable financial actions.

当今支付和 DeFi 是割裂的：支付是静态转账，DeFi 复杂且孤岛化，用户必须手动编排一切。在 Sui 上，这一切将改变——支付可以成为可编程的金融操作。

**示例：**
- 一笔自动投资的支付
- 一笔流式发放并产生收益的工资
- 一个智能路由资金的钱包

Sui 引入了一种根本不同的金融系统构建模型：
- **资产是对象**，不仅是余额
- **交易可以原子化捆绑复杂逻辑**（可编程交易块 PTB）
- **智能合约（Move）** 在类型层面强制所有权和可组合性

这实现了超越传统 DeFi 的能力：**可编程货币**——资产、逻辑和流转原生可组合。

## Prize / 奖金

| Place | Prize |
|-------|-------|
| 1st | $30,000 |
| 2nd | $15,000 |
| 3rd | $10,000 |
| 4th | $7,500 |

## 参考方向 / Suggested Topics

以下方向按类型分组，选一个、改编一个、或者完全忽略——这些只是起点，不是清单。

### 1. 信任最小化系统 / Trust-Minimized Systems

构建通过可编程执行减少或消除各方信任需求的系统。

- 可编程贷款
- 里程碑式托管
- 支付关联信贷系统
- 受控金库管理
- 新型预测市场

### 2. 消费者支付产品 / Consumer Payment Products

关注可用性和真实世界的金融流程。

- 内置自动化的智能钱包
- 商户支付系统
- 订阅或流式支付
- 薪资系统
- 隐私导向的消费支付通道

### 3. 资金管理 / Capital Management

关注可编程资金管理。

- 收益金库
- 自动储蓄策略
- 金库管理系统
- 投资组合分配器

### 4. 自动化与 Agent / Automation & Agents

关注逻辑驱动的执行。

- 自动投资机器人
- 再平衡系统
- 条件支付
- 基于规则的金融 Agent

### 5. 开发者工具 / Developer Tooling

关注赋能其他建设者。

- 支付 SDK
- 交易流构建/可视化工具
- 金融仪表盘
- Move 合约调试工具

## 可用的 Sui 能力 / Available Sui Primitives

| 原语 | 能力 |
|------|------|
| 对象模型 | 安全资产流转、自定义金融规则、可组合模块 |
| PTB（可编程交易块）| 多步支付、复杂金融流（支付→兑换→存款）、无缝用户体验 |
| Coin / NFT | 支付、收据、身份关联金融、代币化仓位 |

可集成的协议：借贷协议、DEX / 流动性场所、收益平台（这些是工具，不是硬性要求）。

## 评估要点 / Evaluation Criteria

**优秀项目需要：**
- 清晰的金融用例
- 正确处理资产和所有权
- 端到端可用的集成/流程
- 对用户的周到抽象

**顶级项目还需要：**
- 对可编程交易的创新使用
- 跨组件的强可组合性
- 复杂金融操作的优秀用户体验
- 真实世界的适用性

> Build something that makes money move smarter.

## 学习资源 / Learning Resources

- [Learning Sui](https://hoh-zone.github.io/learning-sui/) — Sui 中文教程（Move 语言、对象模型、PTB、Coin/NFT 等）
- [DeFi 中文参考文档](https://hoh-zone.github.io/intro-defi) — DeFi 概念与入门
