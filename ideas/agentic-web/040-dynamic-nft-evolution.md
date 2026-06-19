# Dynamic NFT Evolution / 动态 NFT 进化 Agent

> AI 根据持有者行为和环境数据动态调整 NFT 属性和外观

## 解决什么问题

传统 NFT 属性在铸造后固定不变，缺乏与持有者和环境的互动性，限制了 NFT 在游戏、社交和品牌场景中的应用深度。持有者无法通过行为影响 NFT 的价值体现，NFT 与持有者之间缺乏情感连接。本项目通过 AI Agent 实时分析持有者的链上行为和外部环境数据，动态调整 NFT 的属性、等级和外观元数据，赋予 NFT 持续进化的生命力。

## 核心功能

- 追踪 NFT 持有者的链上行为（交易频率、DeFi 参与、社交互动），AI 计算进化因子
- 根据进化因子动态更新 NFT 属性（等级、能力值、稀有度）和外观元数据
- 支持环境事件触发突变进化（如市场剧烈波动、链上里程碑、季节变化）

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `EvolvableNFT` 对象存储当前属性、进化历史和元数据 URL |
| 动态字段 | 为 NFT 附加可变的属性组（等级、能力值、进化轨迹） |
| Walrus | 存储 NFT 动态生成的图像/3D 模型，元数据 URL 指向 Walrus blob |
| 事件（Event） | 发出 `EvolutionEvent` 记录每次属性变更，支持前端实时更新展示 |

## 实现方案

### 智能合约 (Move)
- **`evolvable_nft` 模块**：定义 `EvolvableNFT` 对象，字段包括 `id: ID`、`name: String`、`level: u64`、`xp: u64`、`stage: u8`、`attributes: Bag`、`metadata_url: String`、`owner: address`、`evolved_count: u64`
- **`evolution_engine` 模块**：`trigger_evolution(nft: &mut EvolvableNFT, evolution_type: u8, new_stage: u8, new_metadata: String)` 执行进化
- **`xp_accumulator` 模块**：`add_xp(nft: &mut EvolvableNFT, amount: u64, source: u8)` 积累经验值
- 关键数据结构：`EvolutionRecord { from_stage: u8, to_stage: u8, trigger: u8, timestamp: u64, xp_at_evolution: u64 }`

### 前端 / 后端
- **前端**：React + Three.js/CSS3D，NFT 3D 展示、进化动画、属性雷达图、进化时间线
- **后端**：Python Agent 服务，行为分析引擎、进化决策器、元数据生成器、图像渲染
- 关键功能：NFT 展示柜、进化预览、属性面板、进化历史、排行榜

### AI / Agent 部分
- 行为分析：AI 评估持有者的链上活跃度、DeFi 参与和社交行为，计算经验值
- 进化决策：根据累计 XP 和触发事件决定进化路径，不同行为模式导向不同进化方向
- 视觉生成：Stable Diffusion 根据进化阶段和属性动态生成 NFT 外观图像
- Agent 工作流：行为采集 → XP 计算 → 阈值判断 → 进化触发 → 图像生成 → 元数据更新 → 链上记录

## 技术架构

Agent 服务持续追踪 NFT 持有者的链上行为。行为分析引擎将多维行为转化为经验值，`xp_accumulator` 模块定期更新 NFT 的 XP。当 XP 达到阈值或环境事件触发时，进化决策器根据行为模式和进化规则选择进化方向。Stable Diffusion 生成新外观图像并上传至 Walrus。元数据更新后通过 PTB 调用 `trigger_evolution` 更新链上 `EvolvableNFT` 对象。前端展示进化动画和属性变化。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | NFT 进化状态管理和属性动态更新 |
| 前端 | ⭐⭐⭐⭐ | 3D 展示、进化动画和属性可视化 |
| AI/ML | ⭐⭐⭐⭐ | 行为分析、进化路径决策和图像生成 |
| 集成复杂度 | ⭐⭐⭐ | Walrus 存储、图像生成和服务协调 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
