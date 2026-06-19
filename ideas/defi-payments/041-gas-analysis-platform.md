# Gas 费分析平台 / Gas Analysis Platform

> 分析协议 Gas 消耗模式，帮助项目方优化合约降低用户成本

## 解决什么问题

Gas 费是影响 DeFi 用户体验的关键因素，但项目方往往不知道自己的合约哪些操作消耗 Gas 最多、哪些函数可以优化。缺乏工具化的 Gas 分析手段，优化只能凭经验猜测。Gas 费分析平台通过分析链上交易数据，精确展示协议的 Gas 消耗模式，识别高消耗操作，提供具体的优化建议，帮助项目方降低用户成本。

## 核心功能

- Gas 热力图：按函数、模块和操作类型展示 Gas 消耗分布
- 交易 Gas 排行：找出 Gas 消耗最高的交易和操作
- 优化建议引擎：基于 Gas 模式分析提供具体的代码优化建议
- Gas 趋势追踪：追踪 Gas 消耗随版本变化的趋势，量化优化效果

## 使用的 Sui 原语

| 原语 | 用途 |
|------|------|
| 事件 (Events) | 解析交易事件获取 Gas 使用数据 |
| Move 对象模型 | 分析不同对象操作的 Gas 消耗差异 |
| 动态字段 | 对比使用动态字段 vs 直接字段的 Gas 差异 |
| PTB | 分析 PTB 中多操作组合的 Gas 效率 |
| 共享对象 | 分析共享对象并发访问的 Gas 模式 |

## 实现方案

### 智能合约 (Move)
- `gas_meter` 模块：包装关键操作记录 Gas 消耗
- `benchmark` 模块：提供标准化的 Gas 基准测试套件
- `optimization_report` 模块：生成链上可验证的优化报告
- 关键数据结构：`GasReport { package_id: ID, function_name: String, avg_gas: u64, max_gas: u64, call_count: u32, optimizations: vector<String> }`

### 前端 / 后端
- 技术栈：Next.js + Recharts + Sui TypeScript SDK
- 关键页面：Package Overview（包总览）、Gas Heatmap（Gas 热力图）、Function Analysis（函数分析）、Optimization Suggestions（优化建议）、Trend Chart（趋势图）
- 后端索引所有交易的 Gas 数据并执行分析

## 技术架构

后端索引器扫描 Sui 链上所有交易，提取 Gas 使用数据（计算消耗、存储消耗、总消耗）。按 Package ID 和函数名聚合分析，生成 Gas 消耗热力图和排行。优化引擎基于常见模式（如过多的动态字段访问、不必要的对象创建、可合并的小操作）生成具体建议。项目方可连接自己的 Package ID 查看专属分析报告。版本对比功能帮助量化每次优化的实际 Gas 节省量。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐⭐⭐ | 基准测试和报告模块需要灵活设计 |
| 前端 | ⭐⭐⭐⭐ | Gas 热力图和优化建议展示需要丰富的数据可视化 |
| 集成复杂度 | ⭐⭐⭐ | 需要全面索引链上交易数据和 Gas 信息 |

## 官方参赛要求检查

- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
