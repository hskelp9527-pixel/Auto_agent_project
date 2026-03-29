# Self-Evolution System

基于 autoresearch (Andrej Karpathy) 理念的**文件驱动自主开发系统**。

## 核心理念

AI 通过文件（PRD.md → task.md → progress.md → evaluation.md）沟通协作，每次从 task.md 获取一个任务开发，独立评估器打分，加分确认完成，减分直接重写，迭代直到95分。

**核心特性**：
- 📁 **文件驱动**：AI 之间通过文件沟通，不依赖上下文
- 🔄 **自主迭代**：加分确认，减分重写，持续到95分
- 🤖 **独立评估**：独立 agent 评估打分，结果写 evaluation.md
- 📝 **完整记录**：task.md、progress.md、evaluation.md、ERROR_LOG.md

## 何时使用

- ✅ "帮我开发一个XXX项目"
- ✅ "用 self-evolution-system 从零构建项目"
- ✅ "重构这个项目"
- ✅ "基于 PRD 开发项目"

## 核心流程

```
阶段1：需求确认 → 生成 PRD.md
   ↓
阶段2：任务拆解 → PRD.md → task.md
   ↓
阶段3：自主迭代
   读取 task.md → 获取任务 → 开发 → 评估打分 →
   加分 → 确认完成，更新progress.md
   减分 → 直接重写
   不变 → 继续下一个任务
   ↓ 重复直到95分
阶段4：交付（≥95分）
```

## 文件结构

```
project/
├── PRD.md              # 需求文档
├── task.md             # 任务列表
├── progress.md         # 开发进度
├── evaluation.md       # 评估结果
├── ERROR_LOG.md        # 错误历史
└── [项目代码]
```

## 评分标准

| 维度 | 分数 | 说明 |
|------|------|------|
| 功能完整性 | 60分 | 页面、表单、数据、图表、CRUD、可用性 |
| 设计系统 | 20分 | 依赖、变体、令牌、无障碍、响应式 |
| 代码质量 | 10分 | TypeScript、组件化、错误处理、规范 |
| 功能验证 | 10分 | 启动、导航、交互、响应式 |
| **总分** | **100分** | **目标：≥95分** |

## 关键原则

1. **文件驱动沟通** ⚠️ 最高优先级
   - AI 之间通过文件沟通，不依赖上下文
   - 换个 AI 看着文件也能继续

2. **ERROR_LOG.md 强制使用**
   - 每次开发前必须读取
   - 避免重复错误

3. **迭代改进**
   - 加分确认完成
   - 减分直接重写
   - 持续迭代到95分

4. **独立评估**
   - 独立 agent 评估
   - 结果写 evaluation.md

5. **禁止中间询问**
   - 不要在75分或其他中间分数询问用户
   - 持续迭代到95分

## 参考文档

- `SKILL.md` - 主文档
- `references/evaluation-standard.md` - 评估标准
- `references/common-traps.md` - 常见陷阱
- `references/example-workflow.md` - 示例工作流程
- `references/key-principles.md` - 关键原则

## 理念来源

本系统基于 [autoresearch](https://github.com/karpathy/autoresearch) (Andrej Karpathy) 的核心思想设计。

**autoresearch 原版**：
- 领域：机器学习研究
- 方法：AI 修改 train.py → 训练 → 评估 → 保留或丢弃

**本系统改造**：
- 领域：通用开发项目
- 方法：AI 读取 task.md → 开发 → 评估 → 加分确认/减分重写

## 版本历史

- **v2.0.0** (2025-03-29): **重大重构 - 文件驱动自主开发系统**
  - 从"验证工具"重构为"开发系统"
  - 核心流程：PRD → task → 开发 → 评估 → 迭代
  - 文件驱动：AI 之间通过文件沟通
  - 加分确认，减分重写
  - 95分交付标准

- **v1.3.0** (2025-03-29): 根据官方 skill-creator 优化
  - 拆分文件结构（732→200行）
  - 优化 description 更 pushy

- **v1.2.0** (2025-03-29): 添加第0轮 ERROR_LOG.md 强制检查

- **v1.1.0** (2025-03-29): 初始版本
