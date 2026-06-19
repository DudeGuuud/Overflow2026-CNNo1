# Decentralized AI Inference Market / 去中心化 AI 推理市场

> 将 AI 推理请求和结果上链，通过 Sui 对象模型管理模型版本和付费

## 解决什么问题

AI 推理服务被少数中心化平台垄断，定价不透明，模型版本和推理结果无法被验证和追溯。用户无法确认推理结果是否由声明的模型生成，缺乏信任基础。本项目在 Sui 上构建去中心化 AI 推理市场，推理提供者注册模型和服务，用户提交推理请求并支付费用，推理结果上链存证，通过对象模型管理模型版本和付费流程，确保推理服务透明可验证。

## 核心功能

- 推理提供者注册模型（描述、版本、输入格式、定价），用户按需提交推理请求
- 推理结果和模型元数据上链存证，支持结果验证和历史查询
- 内置付费和激励分配：用户支付费用自动分配给推理提供者和验证者

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `ModelRegistry`、`InferenceRequest`、`InferenceResult` 对象管理推理全生命周期 |
| PTB | 将「创建请求 → 支付费用 → 提交结果 → 验证 → 分配奖励」编排为原子交易 |
| Coin/Treasury | 管理推理费用的支付、托管和分配 |
| 事件（Event） | 发出 `RequestCreatedEvent`、`ResultSubmittedEvent` 驱动推理服务流程 |

## 实现方案

### 智能合约 (Move)
- **`model_registry` 模块**：定义 `AIModel` 对象，字段包括 `model_id: ID`、`name: String`、`version: String`、`provider: address`、`input_schema: vector<u8>`、`price_per_call: u64`、`call_count: u64`、`rating: u64`
- **`inference_request` 模块**：`create_request(model: &AIModel, input_data: vector<u8>, payment: Coin<SUI>): InferenceRequest` 创建付费请求
- **`inference_result` 模块**：`submit_result(request: &mut InferenceRequest, output_data: vector<u8>, proof: vector<u8>)` 提交结果
- 关键数据结构：`InferenceRequest { request_id: ID, model_id: ID, requester: address, input_hash: vector<u8>, payment: u64, status: u8, deadline: u64 }`

### 前端 / 后端
- **前端**：React + TailwindCSS，模型市场、推理请求面板、结果查看器、提供者仪表盘
- **后端**：Rust 推理网关，请求分发、结果收集、验证服务、费用结算
- 关键功能：模型浏览、在线推理、结果验证、费用管理、提供者注册

### AI / Agent 部分
- 模型服务：推理提供者运行 AI 模型服务（支持 OpenAI API 兼容接口），响应链上请求
- 结果验证：轻量级验证机制检查推理结果格式和基本合理性
- 市场匹配：根据请求类型、预算和延迟要求匹配最优推理提供者
- Agent 工作流：用户提交请求 → 链上支付托管 → 匹配提供者 → 执行推理 → 提交结果 → 验证 → 费用释放

## 技术架构

推理提供者将模型元数据注册为 `AIModel` Move 对象。用户通过 PTB 创建 `InferenceRequest` 并支付费用至托管合约。推理网关监听请求事件，将输入数据分发至匹配的提供者。提供者执行推理后通过 PTB 提交结果到 `InferenceResult` 对象。验证服务检查结果合理性后触发费用释放 PTB，将托管资金分配给提供者。前端展示模型市场和推理结果。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐⭐ | 模型注册、请求管理和费用托管机制 |
| 前端 | ⭐⭐⭐ | 模型市场和推理面板 |
| AI/ML | ⭐⭐⭐ | 推理服务和结果验证 |
| 集成复杂度 | ⭐⭐⭐⭐ | 推理网关、服务发现和费用结算 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
