# Portfolio Rebalancer / 投资组合再平衡器

> 用户设定目标配比，AI 定期生成再平衡 PTB，展示预期变化后由用户确认

## 解决什么问题

加密资产价格波动导致投资组合偏离目标配置，用户需要定期手动计算偏差并执行再平衡操作，过程繁琐且容易出错。缺乏链上原生的再平衡工具意味着用户无法自动维持理想的风险敞口分布。本项目让用户设定投资组合目标配比，AI 定期计算偏差并生成再平衡 PTB，展示预期变化后由用户确认执行，帮助用户系统化管理链上资产配置。

## 核心功能

- 创建投资组合：选择多个资产和目标权重（如 SUI 40%、USDC 30%、CETUS 30%）
- AI 定期（每日/每周）计算当前配比与目标的偏差，超过阈值时生成再平衡建议
- 展示再平衡预览：每项资产的当前数量 → 目标数量 → 需要买入/卖出数量 → 预计 Gas
- 一键确认执行再平衡 PTB，所有兑换操作原子化完成

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| PTB | 再平衡操作打包为 PTB：多笔兑换 + 转账，原子化执行 |
| DeepBook | 资产兑换执行，获取最优价格 |
| Move 对象模型 | `PortfolioConfig` 对象存储目标配比和再平衡参数 |
| Coin 管理 | 多币种余额查询和精确兑换数量计算 |

## 实现方案

### 智能合约 (Move)
- **`portfolio_config` 模块**：定义 `PortfolioConfig` 对象，字段包括 `owner: address`、`assets: vector<AssetWeight>`、`rebalance_threshold_bp: u64`、`max_slippage_bp: u64`、`last_rebalance_epoch: u64`、`total_rebalance_count: u64`
- **`rebalance_engine` 模块**：`execute_rebalance(config: &mut PortfolioConfig, trades: vector<Trade>)` 执行再平衡交易序列
- **`portfolio_snapshot` 模块**：`take_snapshot(config: &PortfolioConfig, balances: vector<u256>)` 记录再平衡前后的组合快照
- 关键数据结构：`AssetWeight { coin_type: Type, target_weight_bp: u64 }`、`Trade { from_coin: Type, to_coin: Type, amount: u64 }`

### 前端 / 后端
- **前端**：React + ECharts，资产配置饼图、再平衡预览对比、历史配比趋势图
- **后端**：TypeScript Agent 服务，偏差计算、PTB 生成、定时调度
- 关键功能：组合配置器、再平衡看板、历史记录

### AI / Agent 部分
- 偏差检测：自动计算当前配比与目标配比的偏差百分比
- 最优交易序列：图算法计算最小交易数实现目标配比（减少兑换次数和 Gas）
- 智能建议：GPT-4o 根据市场状况分析是否应调整目标配比
- Agent 工作流：定时扫描用户组合 → 计算偏差 → 超阈值生成 PTB → 展示预览 → 用户确认 → 执行

## 技术架构

用户创建 PortfolioConfig 对象设定资产目标权重。Agent 服务定时（可配置频率）读取用户钱包中各资产余额，计算当前配比与目标的偏差。偏差超过阈值时，Agent 通过图优化算法计算最小兑换交易序列，构造 PTB（多步 DeepBook 兑换），在前端展示预览对比。用户确认后签名提交，PTB 原子化执行所有兑换。PortfolioSnapshot 记录再平衡前后状态，前端展示历史配比演变趋势。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 组合配置和再平衡执行逻辑 |
| 前端 | ⭐⭐⭐ | 配比可视化和再平衡预览 UI |
| AI/ML | ⭐⭐ | 偏差计算和交易序列优化算法 |
| 集成复杂度 | ⭐⭐⭐ | 多币种余额查询和 DeepBook 兑换 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
