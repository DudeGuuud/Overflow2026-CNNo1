# Trusted AI Oracle / 可信 AI 输出预言机

> AI 推理结果通过零知识证明验证后上链，其他合约可安全引用

## 解决什么问题

AI 模型的推理结果无法被链上合约直接验证，合约必须信任外部 AI 服务的输出，这引入了中心化信任风险。恶意 AI 提供者可以篡改推理结果，导致依赖 AI 输出的 DeFi 协议做出错误决策。本项目通过零知识证明技术验证 AI 推理过程的完整性，将验证通过的推理结果上链，使其他 Move 合约可以安全、信任最小化地引用 AI 输出。

## 核心功能

- AI 模型推理结果附带零知识证明，证明推理过程使用了声明的模型和输入
- 链上验证合约验证 ZK 证明的有效性，验证通过后将推理结果写入共享对象
- 其他 Move 合约通过标准接口查询已验证的 AI 输出，构建 AI 驱动的链上逻辑

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| Move 对象模型 | `VerifiedAIOutput` 对象存储经过 ZK 验证的推理结果，可被其他合约引用 |
| 共享对象 | `AIOracleRegistry` 共享对象管理已注册的 AI 模型和验证密钥 |
| PTB | 将「提交证明 → 链上验证 → 写入结果 → 触发回调」编排为原子交易 |
| Seal | 使用 Seal 加密敏感推理输入，仅授权方可解密查看原始数据 |

## 实现方案

### 智能合约 (Move)
- **`ai_oracle` 模块**：定义 `VerifiedAIOutput` 对象，字段包括 `model_id: ID`、`input_hash: vector<u8>`、`output_data: vector<u8>`、`proof_hash: vector<u8>`、`verified: bool`、`verifier: address`、`timestamp: u64`、`consumers: u64`
- **`model_registry` 模块**：`register_model(name: String, model_hash: vector<u8>, verification_key: vector<u8>)` 注册模型
- **`proof_verifier` 模块**：`verify_and_store(proof: vector<u8>, public_input: vector<u8>, model: &AIModel): VerifiedAIOutput` 验证并存储
- 关键数据结构：`AIModel { model_id: ID, name: String, model_hash: vector<u8>, verification_key: vector<u8>, output_count: u64 }`

### 前端 / 后端
- **前端**：React + TailwindCSS，模型注册面板、推理提交界面、验证状态看板、消费者查询面板
- **后端**：Rust ZK 证明生成器、推理网关、验证协调服务
- 关键功能：模型注册、推理请求、ZK 证明生成、链上验证、结果查询

### AI / Agent 部分
- ZK 推理证明：为特定模型（如决策树、小型神经网络）生成推理完整性证明
- 模型注册：AI 提供者注册模型及其验证密钥，支持模型版本管理
- 结果消费：Agent 或合约通过标准接口查询已验证输出，无需信任第三方
- Agent 工作流：推理请求 → 模型执行 → ZK 证明生成 → 链上验证 → 结果存储 → 合约消费

## 技术架构

AI 提供者在 `AIOracleRegistry` 注册模型元数据和 ZK 验证密钥。用户或 Agent 提交推理请求，提供者运行模型生成结果和 ZK 推理证明。证明和结果通过 PTB 提交至 `proof_verifier` 合约。合约使用验证密钥校验 ZK 证明，验证通过后创建 `VerifiedAIOutput` 对象存储结果。其他 Move 合约通过对象引用读取已验证输出，无需信任任何外部方。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐⭐ | 验证逻辑、模型注册和输出管理 |
| 前端 | ⭐⭐⭐ | 模型管理和验证状态面板 |
| AI/ML | ⭐⭐⭐⭐⭐ | ZK 推理证明生成和验证算法 |
| 集成复杂度 | ⭐⭐⭐⭐⭐ | ZK 系统、证明生成管道和链上验证 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
