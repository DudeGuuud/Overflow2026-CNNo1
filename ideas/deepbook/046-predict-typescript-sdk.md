# Predict TypeScript SDK / Predict TypeScript SDK

> TypeScript SDK，方便前端集成 Predict 功能

## 解决什么问题

前端开发者需要在 Web 应用中集成 Predict 功能，但缺少类型安全、文档完善的 TypeScript SDK。Predict TypeScript SDK 提供完整的类型定义和简洁的 API，让前端开发者可以用熟悉的 TypeScript 快速集成 Predict 的 mint、redeem、仓位查询等功能，大幅降低 DApp 开发门槛。

## 核心功能

- 完整类型定义：所有 Predict 合约参数和返回值的 TypeScript 类型
- 简洁 API：`predict.mint()`、`predict.redeem()`、`predict.getPositions()`
- React Hooks：`usePredictPosition`、`useOraclePrice` 等 React 钩子
- 钱包集成：内置 Sui Wallet / Ethos 连接支持

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::mint | TypeScript API 封装 |
| predict::redeem_permissionless | 赎回 API 封装 |
| PredictManager | 仓位查询 API |
| @mysten/sui.js | 底层 Sui 交互 |

## 实现方案

### SDK 核心
- PTB 构建器：TypeScript 函数映射到 Programmable Transaction Block
- 类型系统：完整的 Predict 合约类型定义（ Strike, Expiry, Position, etc.）
- 查询客户端：封装 predict-server API 调用
- 事件订阅器：实时监听仓位变化和结算事件
- 错误处理：统一的错误类型和重试逻辑

### React 集成
- React Hooks 库：常用功能的 React 钩子
- Context Provider：PredictProvider 管理全局状态
- 组件库：可选的 UI 组件（交易卡片、仓位列表）
- Next.js 示例：完整的 Next.js 集成示例

### 开发者工具
- npm 包发布：`npm install @predict/sdk`
- API 文档：TypeDoc 自动生成
- 示例项目：Create React App / Next.js 模板
- 单元测试：Vitest + 覆盖率报告

## 技术架构

开发者安装 npm 包 → 配置 Sui 连接 → 调用 TypeScript API → SDK 内部构建 PTB → 通过 @mysten/sui.js 提交 → 返回类型化结果。React Hooks 自动管理状态和缓存。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐ | 无需自定义合约 |
| SDK 设计 | ⭐⭐⭐⭐ | 类型系统和 API 设计 |
| React 集成 | ⭐⭐⭐ | Hooks 和 Context 设计 |
| 集成复杂度 | ⭐⭐⭐ | PTB 构建和 BCS 编码 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
