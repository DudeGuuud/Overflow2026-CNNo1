# 交易模拟与调试工具 / Transaction Simulator & Debugger

> 在本地模拟 PTB 执行，预览资产变化，调试 Move 合约

## 解决什么问题

开发复杂的 PTB 交易时，开发者难以预判执行结果和 Gas 消耗。直接在链上测试成本高且影响真实资产，错误可能导致资金损失。现有的 Move 单元测试功能有限，无法完整模拟 PTB 的多步执行和跨合约交互。交易模拟与调试工具让开发者在本地完整模拟 PTB 执行，逐步调试每个操作的输入输出，预览资产变化和 Gas 消耗。

## 核心功能

- 本地 PTB 模拟：在本地环境中完整模拟 PTB 执行，无需上链
- 逐步调试：像 IDE 调试器一样逐步执行 PTB，查看每个操作的状态变化
- 资产变化预览：展示执行前后每个地址的资产余额变化
- Gas 估算：精确计算 PTB 执行的 Gas 消耗

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| PTB | 模拟的目标对象，解析和重放 PTB 交易 |
| Move 对象模型 | 模拟器需要完整实现对象所有权和状态管理 |
| Coin 标准 | 模拟代币余额变化 |
| 共享对象 | 模拟共享对象的并发交互 |
| Dry Run | 利用 Sui 的 dry-run 功能进行交易模拟 |

## 实现方案

### 智能合约 (Move)
- `snapshot` 模块：创建链上状态的快照用于模拟
- `mock_oracle` 模块：模拟预言机价格数据
- `mock_protocol` 模块：模拟 DeFi 协议的行为
- 关键数据结构：`SimulationResult { gas_used: u64, balance_changes: vector<BalanceChange>, object_changes: vector<ObjectChange>, events: vector<Event> }`

### 前端 / 后端
- 技术栈：Electron（桌面应用）+ Sui TypeScript SDK + Monaco Editor
- 关键页面：PTB Editor（PTB 编辑器）、Step Debugger（逐步调试器）、Balance View（资产变化视图）、Gas Analyzer（Gas 分析器）
- 后端使用 Sui dry-run API 和本地状态管理

## 技术架构

开发者在编辑器中编写或导入 PTB 交易。模拟器从测试网获取当前链上状态快照，在本地环境中按步骤执行 PTB。逐步调试模式下，开发者可以在每个操作前后暂停，查看对象状态、Coin 余额和事件输出。执行完成后展示完整的资产变化对照表和 Gas 消耗明细。支持设置断点、修改变量值和回退到特定步骤重新执行。模拟结果与真实链上执行高度一致。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 模拟模块需要准确反映链上行为 |
| 前端 | ⭐⭐⭐⭐⭐ | 调试器 UI 是核心，需要出色的开发者体验 |
| 集成复杂度 | ⭐⭐⭐⭐ | 需要深度集成 Sui SDK 的 dry-run 和状态管理 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
