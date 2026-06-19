# Predict Desktop Widget / Predict 桌面组件

> macOS/Windows 桌面小组件，实时显示 BTC 预测价格和你的仓位

## 解决什么问题

Predict 交易者需要频繁查看 BTC 预言机价格和个人仓位状态，但每次都打开浏览器或钱包 App 效率太低。Predict Desktop Widget 提供轻量级桌面小组件（macOS 桌面 Widget / Windows Widget），常驻显示 BTC 预测价格、活跃仓位状态和盈亏，让交易者无需切换应用即可掌握市场动态。

## 核心功能

- 实时 BTC 预言机价格显示（基于 OracleSVI 更新）
- 活跃仓位摘要：当前持仓方向、行权价、到期时间、预估盈亏
- 价格到价提醒：设定价格阈值，触发时桌面通知
- 可定制尺寸和主题：小型/中型/大型 Widget

## 使用的 DeepBook / Sui 能力

| 能力 | 用途 |
|------|------|
| OracleSVI | 获取实时预言机价格 |
| predict-server API | 查询用户仓位和状态 |
| Sui Event API | 监听仓位变化和结算事件 |
| PredictManager | 读取用户仓位信息 |

## 实现方案

### 前端 / 后端
- 技术栈：Swift WidgetKit (macOS) / React + Tauri (跨平台)
- 数据桥接：本地 HTTP Server 提供 predict-server 数据给 Widget
- 仓位解析器：从 predict-server API 获取用户仓位并格式化
- 通知引擎：系统原生通知 API
- 配置面板：设置钱包地址、关注的价格阈值

### Bot / Keeper / 服务端
- 轻量数据服务：代理 predict-server API，添加缓存和聚合
- 仓位监控器：定期查询指定地址的活跃仓位
- 价格告警器：检测到阈值触发时推送通知

## 技术架构

本地数据服务定期从 predict-server 拉取数据 → 解析预言机价格和用户仓位 → 缓存到本地 → Widget 定时刷新读取缓存 → 展示价格和仓位 → 价格触发阈值时系统通知。

## 难度评估

| 维度 | 难度 | 说明 |
|------|------|------|
| Move 合约 | ⭐ | 无需自定义合约 |
| Bot/算法 | ⭐⭐ | 数据代理和缓存 |
| 前端 | ⭐⭐⭐⭐ | 原生 Widget 开发需要平台特定技能 |
| 集成复杂度 | ⭐⭐ | 主要依赖 predict-server API |

## 合格要求

- 集成 DeepBook Predict 合约（测试网）
- 端到端可用 / 有模拟结果
- [ ] 项目名称和描述
- [ ] 项目 Logo (1:1)
- [ ] 公开 GitHub 仓库
- [ ] Demo 视频 (≤5min)
- [ ] 测试网部署 + Package ID
