# Predict Python SDK / Predict Python SDK

> Python 封装 Predict 合约调用，支持策略回测和自动化交易

## 解决什么问题

量化交易者和数据科学家习惯使用 Python 构建策略，但 Sui/Move 生态缺少 Python 原生的 Predict SDK。Predict Python SDK 提供简洁的 Python API 封装 Predict 合约调用，支持策略开发、历史数据回测和自动化交易。开发者可以用熟悉的 Python 工具链（pandas、numpy、scikit-learn）快速构建 Predict 策略。

## 核心功能

- 简洁的 Python API：`predict.mint(strike=70000, direction="up", amount=100, expiry="15m")`
- 历史数据接口：获取预言机价格、SVI 参数、交易历史的 DataFrame
- 策略回测框架：基于历史数据模拟策略执行和盈亏计算
- 异步支持：async/await 高并发交易

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::mint | Python API 封装交易执行 |
| predict::redeem_permissionless | 赎回接口封装 |
| OracleSVI | 提供历史数据查询 |
| Sui RPC / predict-server | 链上交互和数据获取 |

## 实现方案

### SDK 核心
- 连接管理器：Sui RPC 连接、钱包管理、Gas 配置
- 交易构建器：Python 函数映射到 PTB 构建
- 数据客户端：从 predict-server 获取历史数据的 pandas DataFrame
- 回测引擎：模拟执行策略并计算 Sharpe/最大回撤等指标
- 事件处理器：异步订阅链上事件

### 开发者工具
- pip 包发布：`pip install predict-sdk`
- Jupyter Notebook 示例：策略开发和可视化
- CLI 工具：命令行查询和交易
- 文档：API Reference + 教程 + 示例策略

## 技术架构

开发者安装 pip 包 → 配置 Sui 连接和钱包 → 调用 Python API → SDK 内部构建 PTB → 通过 Sui RPC 提交交易 → 结果返回 Python 对象。数据接口返回 pandas DataFrame 便于分析。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐ | 无需自定义合约 |
| SDK 设计 | ⭐⭐⭐⭐ | API 设计和 PTB 封装 |
| 回测框架 | ⭐⭐⭐⭐ | 历史数据模拟和指标计算 |
| 集成复杂度 | ⭐⭐⭐ | BCS 编码和 Sui RPC 交互 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
