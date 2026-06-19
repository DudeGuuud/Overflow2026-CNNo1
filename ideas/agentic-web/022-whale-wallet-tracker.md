# Whale Wallet Tracker / whale 钱包追踪器

> AI 识别和标记大户地址，追踪其链上操作，生成实时告警和策略信号

## 解决什么问题

大户（whale）的链上操作对市场价格和流动性有显著影响，但普通用户缺乏有效工具追踪大户行为。手动监控特定地址效率低，且难以从海量交易中识别出真正重要的大户操作。本项目利用 AI 自动识别 Sui 生态中的大户地址，追踪其链上操作（大额转账、DEX 交易、协议交互），在关键操作发生时实时告警并生成可参考的策略信号。

## 核心功能

- 自动识别大户地址：基于持仓量、交易频率和单笔金额等指标智能标记 whale 地址
- 实时追踪大户操作：大额转账、DEX 买卖、流动性变动、协议存取款
- 告警系统：关键操作实时推送（链上 Event + 链下通知），可配置关注列表和告警阈值
- 策略信号：AI 分析大户操作模式，生成"whale 看多/看空"等情绪信号

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `WhaleRegistry` 共享对象维护大户地址列表和标签 |
| 事件（Event） | 发出 `WhaleAlertEvent` 在大户关键操作时实时通知 |
| 链上数据 | 读取 Coin 余额、对象所有权分析大户持仓和操作 |
| 动态字段 | 为每个大户地址附加标签、持仓快照和行为模式 |

## 实现方案

### 智能合约 (Move)
- **`whale_registry` 模块**：定义 `WhaleRegistry` 共享对象，字段包括 `whale_count: u64`、`whale_addresses: vector<address>`、`last_update: u64`
- **`whale_tag` 模块**：`tag_whale(registry: &mut WhaleRegistry, addr: address, label: String, tier: u8)` 标记大户地址并分类（mega/large/medium whale）
- **`alert_module` 模块**：`emit_whale_alert(registry: &WhaleRegistry, addr: address, action: u8, amount: u256, target: address)` 发出大户操作告警
- 关键数据结构：`WhaleProfile { addr: address, tier: u8, total_holding: u256, primary_protocols: vector<ID>, last_active: u64, label: String }`

### 前端 / 后端
- **前端**：React + TailwindCSS，大户排行榜、实时操作流、持仓分析图表
- **后端**：Python Agent 服务，地址分类引擎、交易监控管道、信号生成器
- 关键功能：大户搜索、实时告警流、持仓变化追踪

### AI / Agent 部分
- 大户识别：聚类算法（DBSCAN）基于持仓量和交易模式自动识别大户地址
- 操作分类：Transformer 模型分析大户交易序列，判断操作类型（建仓、减仓、套利、迁移）
- 信号生成：AI 综合多个大户的近期操作方向，生成 whale 情绪指标（-100 到 +100）
- Agent 工作流：每日扫描地址余额 → 更新大户列表 → 实时监控交易 → 关键操作告警 → 生成策略信号

## 技术架构

Agent 服务通过 Sui RPC 每日扫描高余额地址列表，DBSCAN 聚类识别并分级大户。实时监控管道订阅大户地址的交易事件，Transformer 模型分类操作类型。关键操作（大额买卖、协议大额存取）触发 WhaleAlertEvent 上链并推送前端告警。多维度信号引擎聚合大户行为方向，输出 whale 情绪指标。前端提供大户排行榜、实时操作流和持仓追踪视图。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 大户注册表和告警事件 |
| 前端 | ⭐⭐⭐ | 实时数据流展示和排行榜 UI |
| AI/ML | ⭐⭐⭐⭐ | 大户识别聚类和操作分类模型 |
| 集成复杂度 | ⭐⭐⭐⭐ | 全量地址扫描和实时交易监控管道 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
