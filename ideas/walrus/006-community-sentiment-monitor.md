# Community Sentiment Monitor / 社区情绪监控

> 追踪 Discord/Telegram 社区情绪变化，历史数据存储在 Walrus，生成情绪趋势图 / Tracks Discord/Telegram sentiment changes with historical data in Walrus and generates sentiment trend charts

## 解决什么问题

Web3 项目的社区情绪是项目健康度的重要指标，但目前缺乏长期、系统化的情绪追踪工具。项目方需要了解社区对产品更新、代币经济、治理提案等事件的反应变化，以便及时调整策略。历史情绪数据还能用于预测社区行为。

## 核心功能

- 多频道情绪采集：接入 Discord/Telegram 频道，实时分析消息情绪（积极/消极/中性）
- 关键词热度追踪：监控特定关键词的出现频率和关联情绪
- 情绪趋势报告：每日/每周生成情绪变化趋势报告，标注关键事件触发点
- 异常预警：情绪突然大幅波动时自动通知项目管理团队

## 使用的 Walrus / Sui 能力

| 能力 | 用途 |
|------|------|
| Walrus Blob 存储 | 存储情绪分析原始数据（JSON）、趋势报告（Markdown）、聚合统计 |
| MemWal 记忆层 | 记录历史情绪基线和异常事件，用于新数据的对比分析 |
| Sui 对象模型 | 链上记录监控配置、频道元数据、报告索引 |
| Walrus 读取 API | 按日期范围检索历史情绪数据 |

## 实现方案

### Agent / AI 部分
- 使用 LangChain 构建 Sentiment Agent 工作流
- 情绪分析使用多模型策略：GPT-4o-mini 做快速分类，复杂语境用 Claude 3.5 判断
- 关键词提取使用 YAKE 算法 + LLM 辅助
- 时间序列异常检测使用 Isolation Forest 算法

### Walrus 集成
- 每小时情绪快照以 `sentiment/{channel}/{hour}.json` 存储
- 每日趋势报告以 `reports/{project}/{date}.md` 存储
- 关键词热力图数据以 `keywords/{project}/{date}.json` 存储
- Sui 链上 SentimentIndex 对象管理所有 Blob 的日期索引

### 前端 / 后端
- 前端：React + D3.js，展示情绪趋势图和关键词热力图
- 后端：Python FastAPI，WebSocket 实时推送情绪变化
- 关键页面：实时情绪仪表盘、趋势分析页、关键词追踪页、告警配置页

## 技术架构

Discord/Telegram Bot 监听消息 → 消息队列（Redis Streams）缓冲 → Sentiment Agent 批量分析 → 情绪评分和关键词存入 Walrus Blob → 每日汇总报告也写入 Walrus → Sui 链上更新索引 → 前端查询索引获取 Blob → 渲染趋势图。异常检测 Agent 实时比对 MemWal 中的历史基线。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Walrus 集成 | ⭐⭐ | 高频写入 + 每日汇总的标准模式 |
| AI/Agent | ⭐⭐⭐ | 情绪分析准确度和多语言支持 |
| 前端 | ⭐⭐⭐ | 实时图表和交互式趋势展示 |
| 集成复杂度 | ⭐⭐⭐ | 多平台 Bot + 消息队列 + 分析流水线 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
