# PTB 可视化编辑器 / PTB Visual Editor

> 拖拽构建复杂金融流程，自动生成 Move 代码，支持模拟运行

## 解决什么问题

Programmable Transaction Block (PTB) 是 Sui 的核心特性，允许将多个操作原子化组合执行。但构建复杂 PTB 需要深入了解 Move 语法和 Sui SDK，门槛很高。PTB 可视化编辑器让开发者通过拖拽方式组合金融操作（转账、兑换、借贷等），自动生成可执行的代码，并提供本地模拟运行功能，大幅降低 PTB 开发门槛。

## 核心功能

- 拖拽式构建：将预定义的金融操作块（Swap、Transfer、Deposit、Borrow 等）拖入画布
- 参数可视化配置：每个操作块的参数通过表单配置，无需手写代码
- 代码自动生成：画布布局自动生成对应的 Move/TypeScript PTB 代码
- 模拟运行：在本地 devnet 模拟执行 PTB，预览资产变化和 Gas 消耗

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| PTB | 编辑器的目标输出，生成标准 PTB 代码 |
| Coin 标准 | 操作块支持所有 Coin<T> 类型 |
| Move 对象模型 | 操作块处理对象的所有权和转移 |
| 共享对象 | 模拟运行中测试共享对象交互 |
| DeepBook/DEX | 操作块封装 DEX 交互逻辑 |

## 实现方案

### 智能合约 (Move)
- `ptb_templates` 模块：预定义常用 PTB 模板（swap+transfer、borrow+swap+repay 等）
- `simulation` 模块：在测试环境模拟 PTB 执行结果
- `custom_operation` 模块：注册自定义操作块供编辑器使用
- 关键数据结构：`PTBTemplate { name: String, operations: vector<OperationDef>, parameters: vector<ParamDef> }`

### 前端 / 后端
- 技术栈：React + React Flow（流程图库）+ Monaco Editor（代码编辑器）+ Sui TypeScript SDK
- 关键页面：Editor Canvas（可视化编辑画布）、Code Preview（代码预览）、Simulation Panel（模拟运行面板）、Template Library（模板库）
- 后端提供代码生成和模拟运行服务

## 技术架构

开发者从模板库中选择操作块（如 Swap、Transfer、Deposit），拖入画布中连接成流程图。每个操作块通过表单配置参数（代币类型、金额、目标地址等）。编辑器根据画布布局和参数自动生成对应的 TypeScript PTB 代码，在 Monaco Editor 中实时预览。开发者可点击"模拟运行"，后端在 devnet 上 dry-run PTB，返回预览结果（资产变化、Gas 消耗、潜在错误）。确认无误后一键部署执行。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 模板和模拟模块需要灵活的接口设计 |
| 前端 | ⭐⭐⭐⭐⭐ | 可视化编辑器是核心，需要出色的 UX |
| 集成复杂度 | ⭐⭐⭐⭐ | 需要封装所有常用 Sui 操作为可拖拽组件 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
