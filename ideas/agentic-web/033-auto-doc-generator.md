# Auto Doc Generator / 合约文档自动生成

> 从 Move 代码自动生成中文/英文 API 文档和使用示例

## 解决什么问题

Sui 生态大量 Move 合约缺少完善的文档，开发者不得不直接阅读源码理解接口，效率低下。手动编写文档耗时且容易与代码脱节，合约更新后文档往往未同步更新。本项目通过 AI 深度分析 Move 源码，自动生成结构化的中英文 API 文档，包含函数说明、参数描述、使用示例和注意事项，并随代码更新自动同步。

## 核心功能

- 解析 Move 包结构和函数签名，AI 自动生成中英文 API 文档（Markdown 格式）
- 根据函数逻辑自动生成 PTB 调用示例代码（TypeScript/Python），附带注释说明
- 文档版本管理：合约更新时自动检测接口变更，AI 增量更新文档并标注变更点

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `DocRegistry` 对象管理文档版本，记录 Package ID 和文档哈希 |
| 链上数据 | 读取已部署 Package 的模块接口，确保文档与链上代码一致 |
| 动态字段 | 为每个模块和函数附加文档条目和示例代码 |
| 事件（Event） | 发出 `DocUpdatedEvent` 通知订阅者文档已更新 |

## 实现方案

### 智能合约 (Move)
- **`doc_registry` 模块**：定义 `DocRegistry` 对象，字段包括 `package_id: ID`、`version: u64`、`doc_hash: vector<u8>`、`module_count: u64`、`function_count: u64`、`maintainer: address`
- **`doc_entry` 模块**：`add_module_doc(registry: &mut DocRegistry, module: String, doc_hash: vector<u8>)` 记录模块文档
- **`version_control` 模块**：`update_version(registry: &mut DocRegistry, new_package_id: ID, changelog: String)` 管理文档版本
- 关键数据结构：`DocEntry { module: String, function: String, doc_hash: vector<u8>, lang: u8, updated_at: u64 }`

### 前端 / 后端
- **前端**：React + Docusaurus/VitePress，生成文档网站，支持搜索、多语言切换、版本对比
- **后端**：Python Agent 服务，Move 解析器、AI 文档生成、示例代码生成
- 关键功能：包上传、文档预览、多语言输出、版本对比、导出 Markdown/PDF

### AI / Agent 部分
- 代码理解：GPT-4o 深度分析 Move 函数实现逻辑，理解业务语义而非仅转述签名
- 文档生成：根据函数逻辑生成结构化文档，包含用途说明、参数描述、返回值、注意事项
- 示例生成：根据函数签名和对象依赖自动生成 PTB 调用示例，附带详细注释
- Agent 工作流：包解析 → 模块提取 → 逐函数分析 → 文档生成 → 示例生成 → 校验 → 链上注册

## 技术架构

用户上传 Move 包源码或输入已部署 Package ID，解析器提取完整的模块结构和函数签名。Agent 将每个函数的完整实现代码输入 GPT-4o，生成中英文结构化文档。示例代码生成器根据函数签名和对象类型自动构造 PTB 调用代码。文档校验器检查生成内容与链上接口的一致性。最终文档哈希写入 `DocRegistry` Move 对象上链存证，前端渲染为可搜索的文档网站。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐ | 文档注册和版本管理 |
| 前端 | ⭐⭐⭐ | 文档网站生成和多语言展示 |
| AI/ML | ⭐⭐⭐ | Move 代码理解和文档/示例生成 |
| 集成复杂度 | ⭐⭐ | Move 解析和文档格式转换 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
