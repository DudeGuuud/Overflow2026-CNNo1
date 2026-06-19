# Gas Optimize Keeper / Gas 优化 Keeper

> 智能批量处理多个赎回请求到一个 PTB，降低 Gas 成本

## 解决什么问题

Predict 的大量赎回操作如果逐笔提交，Gas 成本会显著侵蚀 Keeper 和用户收益。Gas Optimize Keeper 通过智能批量处理，将多个 redeem_permissionless 调用打包为一个 PTB，利用 Sui 的交易合并机制大幅降低每笔赎回的平均 Gas 成本。结合 Gas 价格预测和网络拥堵分析，选择最优提交时机。

## 核心功能

- 批量赎回：将多个 redeem_permissionless 打包到一个 PTB 中
- 智能分批：根据 PTB 大小限制和 Gas 预算动态分批
- Gas 时机优化：监控网络 Gas 价格，低拥堵时段批量提交
- 成本报告：每笔赎回的平均 Gas 成本和节省金额

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| predict::redeem_permissionless | 核心赎回操作，批量打包 |
| PTB | 多个赎回调用合并为一个交易 |
| Sui Gas 市场 | 监控 Gas 价格选择提交时机 |
| PredictManager | 扫描可赎回仓位 |

## 实现方案

### Bot / Keeper / 服务端
- 仓位扫描器：定期扫描所有可赎回的已结算仓位
- 批量构建器：将多个赎回操作组装为单个 PTB
- 分批优化器：根据 PTB 大小限制和 Gas 预算计算最优分批策略
- Gas 价格预测器：基于历史数据预测低 Gas 时段
- 提交调度器：在最优时段提交批量 PTB

### 前端 / 后端
- 技术栈：React + Node.js + Redis
- 批量管理面板：当前队列、分批策略、预计节省
- Gas 成本分析：历史 Gas 趋势和节省统计
- 自动化配置：批量大小、Gas 上限、提交频率

## 技术架构

仓位扫描器发现可赎回仓位 → 加入待处理队列 → 批量构建器组装 PTB → 分批优化器计算最优分批 → Gas 预测器等待低 Gas 时段 → 提交调度器批量提交 → 前端展示成本节省。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐ | 无需自定义合约 |
| Bot/算法 | ⭐⭐⭐ | 批量优化和 Gas 预测 |
| 前端 | ⭐⭐ | 成本分析面板 |
| 集成复杂度 | ⭐⭐⭐ | PTB 构建和 Gas 优化 |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
