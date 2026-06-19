# Walrus Data Indexing Service / Walrus 数据索引服务

> 为 Walrus 上的数据建立全文搜索和元数据索引，让海量 Blob 数据可被高效检索和查询

## 解决什么问题

Walrus 上存储的大量 Blob 数据缺乏统一的索引和检索机制，用户无法快速找到所需内容。随着 Agent 持续写入数据，Walrus 上的数据量呈指数增长，但没有类似搜索引擎的能力来定位特定 Blob。本方案为 Walrus 构建去中心化的索引层，支持全文搜索、元数据过滤和语义检索。

## 核心功能

- 全文搜索引擎：对 Walrus 上的文本类 Blob 建立倒排索引，支持关键词和短语搜索
- 元数据索引：提取并索引 Blob 的结构化元数据（类型、时间、标签、作者等），支持多维度过滤
- 语义检索：通过 Embedding 向量化 Blob 内容，支持相似度搜索和语义匹配

## 使用的 Walrus / Sui 能力

| 能力 | 用途 |
|------|------|
| Walrus Blob 读取 | 批量读取 Blob 内容进行索引构建 |
| Walrus Blob 存储 | 存储索引文件和元数据缓存 |
| Sui 对象模型 | 链上记录索引注册信息、数据源映射、查询统计 |
| Sui 事件机制 | 监听新 Blob 上链事件触发增量索引更新 |

## 实现方案

### Agent / AI 部分

- 爬取 Agent 定期扫描 Sui 链上的新 Blob 创建事件，发现新增数据
- 解析 Agent 根据文件类型自动解析 Blob 内容（文本、JSON、PDF、Markdown 等）
- Embedding Agent 使用嵌入模型将内容向量化，生成语义索引
- 分类 Agent 自动为 Blob 打标签和分类，丰富元数据索引
- 查询理解 Agent 解析用户自然语言查询，转换为结构化搜索条件

### Walrus 集成

- 倒排索引以 `index/{collection}/inverted.json` 格式存储在 Walrus
- 向量索引以 `index/{collection}/vectors.bin` 格式存储
- 元数据索引以 `index/{collection}/metadata.json` 格式存储
- 索引配置以 Sui 链上对象存储，定义索引规则和更新频率
- 增量日志以 `index/{collection}/changelog.json` 记录索引变更

### 前端 / 后端

- 前端：React 搜索界面，支持关键词搜索、过滤器和结果预览
- 后端：Python 索引服务 + Rust 高性能查询引擎
- 关键页面：搜索页、索引管理页、数据源配置页、搜索统计页

## 技术架构

Sui 链上监听新 Blob 事件 → 爬取 Agent 获取 Blob ID 列表 → 从 Walrus 下载 Blob 内容 → 解析 Agent 提取文本和元数据 → Embedding Agent 生成向量 → 倒排索引和向量索引更新 → 索引文件写回 Walrus → 用户发起搜索 → 查询理解 Agent 解析意图 → 同时查询倒排索引和向量索引 → 合并排序结果 → 返回匹配的 Blob 列表和内容摘要。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Walrus 集成 | ⭐⭐⭐⭐ | 大规模 Blob 批量读取和索引存储管理 |
| AI/Agent | ⭐⭐⭐ | Embedding 生成和查询理解 |
| 前端 | ⭐⭐ | 搜索界面和结果展示 |
| 集成复杂度 | ⭐⭐⭐⭐ | 索引构建、增量更新、高性能查询 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
