# One-Click Deploy Template / 一键部署策略模板

> CLI 工具，从模板快速部署金库策略到测试网

## 解决什么问题

开发者想快速部署 Predict 策略，但需要处理繁琐的项目初始化——Move 合约框架、前端脚手架、测试配置、部署脚本。One-Click Deploy Template 提供 CLI 工具和预构建模板，开发者选择策略类型后自动生成完整项目（Move 合约 + Bot + 前端），一条命令部署到测试网，让开发者在 5 分钟内从想法到可运行的策略。

## 核心功能

- CLI 工具：`predict init --template range-vault` 初始化项目
- 多策略模板：区间金库、做市、套利、跟单等预设模板
- 自动配置：Move 合约框架、测试、部署脚本自动生成
- 一键部署：`predict deploy --network testnet` 部署到测试网

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::mint | 模板中的交易逻辑 |
| predict::supply | 金库模板的供应逻辑 |
| PredictManager | 模板中的仓位管理 |
| Sui Move 框架 | 合约模板和部署 |

## 实现方案

### CLI 工具
- 模板引擎：基于 Handlebars/EJS 的代码生成
- 策略模板库：
  - `range-vault`：区间阶梯金库
  - `mm-bot`：做市机器人
  - `arb-bot`：套利机器人
  - `copy-trade`：跟单策略
  - `custom`：空模板
- 项目结构生成：Move 合约、Bot 代码、前端、测试、CI/CD
- 部署管理：编译、测试、发布到测试网的自动化流程
- 配置管理：环境变量、网络配置、密钥管理

### 模板内容
- Move 合约：策略核心逻辑 + 测试用例
- Bot 框架：事件监听、策略执行、风控
- 前端骨架：React + 监控面板
- 文档：README + API 文档模板
- CI/CD：GitHub Actions 自动测试和部署

## 技术架构

开发者运行 CLI 选择模板 → 模板引擎生成项目代码 → 开发者在模板基础上修改策略逻辑 → `predict build` 编译 Move 合约 → `predict test` 运行测试 → `predict deploy` 发布到测试网 → 前端自动部署。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 需要编写多种策略模板 |
| CLI 工具 | ⭐⭐⭐ | 代码生成和部署自动化 |
| 模板设计 | ⭐⭐⭐⭐ | 需要设计可扩展的模板架构 |
| 集成复杂度 | ⭐⭐⭐⭐ | 需要覆盖完整的开发到部署流程 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
