# Onchain Social Credit Score / 链上社交信用评分

> 基于链上行为（交易历史、NFT 持有、DAO 参与）AI 生成去中心化信用分

## 解决什么问题

Web3 缺乏标准化的信用评估体系，用户无法通过链上行为积累信用记录，DeFi 借贷依赖超额抵押导致资金效率低下。传统信用评分机构中心化、不透明，无法捕捉链上丰富的行为数据。本项目利用 AI 分析用户在 Sui 上的链上行为，包括交易历史、NFT 持有、DAO 参与度和协议交互，生成去中心化信用评分，评分逻辑链上透明可审计。

## 核心功能

- 多维链上行为采集：交易频率与规模、DeFi 协议交互、NFT 收藏活跃度、DAO 治理参与、链上时长
- AI 信用模型综合评估生成信用评分（300-850），按维度展示评分组成和变化趋势
- 信用分授权机制：用户可选择性地向借贷协议展示信用分，获取更优借贷条件

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `CreditScore` 对象归属用户，存储评分、维度分数和更新时间 |
| 动态字段 | 为不同评估维度（交易、DeFi、NFT、DAO）附加子评分和证据数据 |
| zkLogin | 用户通过社交账号（Google/GitHub）关联链上身份，增强身份可信度 |
| 对象所有权 | 信用评分对象由用户拥有，用户控制谁可以查看评分详情 |

## 实现方案

### 智能合约 (Move)
- **`credit_score` 模块**：定义 `CreditScore` 对象，字段包括 `owner: address`、`score: u64`、`tx_score: u64`、`defi_score: u64`、`nft_score: u64`、`dao_score: u64`、`updated_at: u64`、`history_count: u64`
- **`score_authorization` 模块**：`authorize_viewer(score: &CreditScore, viewer: address, duration_epochs: u64)` 授权第三方查看评分
- **`dimension_evidence` 模块**：`attach_evidence(score: &mut CreditScore, dimension: u8, evidence_hash: vector<u8>)` 附加维度证据
- 关键数据结构：`ScoreDimension { dimension_type: u8, score: u64, weight: u64, evidence_count: u64 }`

### 前端 / 后端
- **前端**：React + Recharts，信用分雷达图、维度详情、历史趋势、授权管理面板
- **后端**：Python Agent 服务，链上数据采集、特征工程、评分模型、API 网关
- 关键功能：评分概览、维度分析、授权管理、评分对比、借贷条件推荐

### AI / Agent 部分
- 特征工程：从链上行为中提取 50+ 特征（交易多样性、DeFi APY 敏感度、NFT 稀有度偏好等）
- 评分模型：XGBoost 模型结合行为特征计算各维度评分，加权汇总为总分
- 趋势预测：LSTM 模型基于历史行为趋势预测未来评分变化
- Agent 工作流：地址数据采集 → 特征提取 → 维度评分计算 → 总分汇总 → 链上更新 → 趋势分析

## 技术架构

Agent 服务定期通过 Sui RPC 和索引服务采集目标地址的链上行为数据。特征工程管道将原始数据转化为行为特征矩阵。XGBoost 评分模型计算交易活跃度、DeFi 参与度、NFT 收藏力和 DAO 治理贡献四个维度评分，加权汇总为总信用分。评分结果通过 PTB 更新 `CreditScore` Move 对象。用户通过前端查看评分详情，并可向借贷协议授权查看以获取更优条件。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 评分对象管理和授权机制 |
| 前端 | ⭐⭐⭐ | 雷达图、趋势图和授权面板 |
| AI/ML | ⭐⭐⭐⭐ | 行为特征工程和评分模型训练 |
| 集成复杂度 | ⭐⭐⭐ | 多源链上数据采集和特征聚合 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
