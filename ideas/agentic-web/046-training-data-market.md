# Training Data Market / 模型训练数据市场

> 基于对象模型的数据集交易市场，AI 训练任务通过 PTB 管理数据授权和支付

## 解决什么问题

AI 模型训练需要大量高质量数据，但数据获取面临版权不清、定价困难和交易信任等问题。数据提供者担心数据被无偿使用，购买者无法提前验证数据质量。现有数据市场中心化运营，手续费高且缺乏透明度。本项目基于 Sui 对象模型构建去中心化训练数据市场，数据集以对象形式注册，通过 PTB 管理数据授权、付费和使用追踪，确保数据交易透明可追溯。

## 核心功能

- 数据提供者上传数据集元数据（描述、大小、格式、样本预览），设定价格和授权条款
- 购买者通过 PTB 完成支付并获取数据访问授权，支持按次付费和订阅模式
- 数据使用追踪：记录每个数据集被哪些训练任务使用，提供者可查看使用统计

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `Dataset`、`AccessLicense`、`UsageRecord` 对象管理数据集全生命周期 |
| PTB | 将「验证授权 → 支付费用 → 授予访问 → 记录使用」打包为原子交易 |
| Walrus | 数据集实际文件存储在 Walrus 去中心化存储，对象中保存 blob 引用 |
| Coin/Treasury | 管理数据集购买的支付、平台手续费和提供者收益 |

## 实现方案

### 智能合约 (Move)
- **`dataset` 模块**：定义 `Dataset` 对象，字段包括 `name: String`、`description: String`、`size_bytes: u64`、`format: String`、`walrus_blob_id: String`、`price: u64`、`license_type: u8`、`provider: address`、`download_count: u64`、`rating: u64`
- **`access_license` 模块**：`purchase_access(dataset: &Dataset, payment: Coin<SUI>): AccessLicense` 购买访问授权
- **`usage_record` 模块**：`record_usage(license: &AccessLicense, task_id: ID, purpose: String)` 记录使用
- 关键数据结构：`AccessLicense { license_id: ID, dataset_id: ID, licensee: address, expires_at: u64, usage_count: u64 }`

### 前端 / 后端
- **前端**：React + Next.js，数据集市场、详情页、购买面板、提供者仪表盘、使用统计
- **后端**：TypeScript 服务，Walrus 上传/下载、元数据索引、推荐引擎、API 网关
- 关键功能：数据集浏览、样本预览、购买下载、上传管理、使用追踪

### AI / Agent 部分
- 数据质量评估：AI 分析数据集样本，生成质量报告（完整性、多样性、标注质量）
- 智能推荐：根据模型训练需求推荐匹配的数据集，评估数据与任务的适配度
- 定价建议：基于数据集特征和市场供需，AI 建议合理定价区间
- Agent 工作流：提供者上传 → 质量评估 → 元数据注册 → 市场展示 → 购买支付 → 授权发放 → 使用记录

## 技术架构

数据提供者将数据文件上传至 Walrus 获取 blob ID，填写元数据后创建 `Dataset` Move 对象。AI 自动分析样本数据生成质量报告附加到对象。购买者浏览市场找到目标数据集，通过 PTB 支付费用并获取 `AccessLicense` 对象。授权对象包含 Walrus blob ID 和解密密钥，购买者据此下载数据。每次使用通过 `UsageRecord` 记录，提供者可查看使用统计和收益。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 数据集对象、授权管理和支付托管 |
| 前端 | ⭐⭐⭐ | 数据市场、预览面板和统计仪表盘 |
| AI/ML | ⭐⭐⭐ | 数据质量评估和推荐引擎 |
| 集成复杂度 | ⭐⭐⭐ | Walrus 存储、文件加密和授权流程 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
