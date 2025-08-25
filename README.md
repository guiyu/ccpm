# Claude Code PM

[![Automaze](https://img.shields.io/badge/By-automaze.io-4b3baf)](https://automaze.io)
&nbsp;
[![Claude Code](https://img.shields.io/badge/+-Claude%20Code-d97757)](https://github.com/automazeio/ccpm/blob/main/README.md)
[![GitHub Issues](https://img.shields.io/badge/+-GitHub%20Issues-1f2328)](https://github.com/automazeio/ccpm)
&nbsp;
[![MIT License](https://img.shields.io/badge/License-MIT-28a745)](https://github.com/automazeio/ccpm/blob/main/LICENSE)
&nbsp;
[![Follow on 𝕏](https://img.shields.io/badge/𝕏-@aroussi-1c9bf0)](http://x.com/intent/follow?screen_name=aroussi)
&nbsp;
[![Star this repo](https://img.shields.io/badge/★-Star%20this%20repo-e7b10b)](https://github.com/automazeio/ccpm)

### 使用规范驱动开发、GitHub Issues、Git worktrees 和并行运行的多个 AI agents 的 Claude Code 工作流，不是更快交付，而是_更好地_交付。

停止丢失上下文。停止在任务上阻塞。停止交付 bugs。这个经过实战验证的系统将 PRD 转化为 epics，将 epics 转化为 GitHub issues，将 issues 转化为生产代码——每一步都有完整的可追溯性。

![Claude Code PM](screenshot.webp)

## 目录

- [背景](#background)
- [工作流](#the-workflow)
- [差异点](#what-makes-this-different)
- [为什么使用 GitHub Issues？](#why-github-issues)
- [核心原则：杜绝即兴编程](#core-principle-no-vibe-coding)
- [系统架构](#system-architecture)
- [工作流阶段](#workflow-phases)
- [命令参考](#command-reference)
- [并行执行系统](#the-parallel-execution-system)
- [关键特性与优势](#key-features--benefits)
- [经过验证的效果](#proven-results)
- [示例流程](#example-flow)
- [立即开始](#get-started-now)
- [本地 vs 远程](#local-vs-remote)
- [技术说明](#technical-notes)
- [支持这个项目](#support-this-project)

## 背景

每个团队都面临着同样的问题：
- **上下文在会话间消失**，迫使不断重新发现
- **并行工作产生冲突**，当多个开发者接触同一代码时
- **需求漂移**，口头决策覆盖书面规范
- **进度不可见**，直到最后才能看到

这个系统解决了所有这些问题。

## 工作流

```mermaid
graph LR
    A[PRD 创建] --> B[Epic 规划]
    B --> C[任务分解]
    C --> D[GitHub 同步]
    D --> E[并行执行]
```

### 实际演示（60 秒）

```bash
# 通过引导式头脑风暴创建全面的 PRD
/pm:prd-new memory-system

# 将 PRD 转换为带有任务分解的技术 epic
/pm:prd-parse memory-system

# 推送到 GitHub 并开始并行执行
/pm:epic-oneshot memory-system
/pm:issue-start 1235
```

## 差异点

| 传统开发方式 | Claude Code PM 系统 |
|------------------------|----------------------|
| 会话间上下文丢失 | **持久化上下文** 贯穿所有工作 |
| 串行任务执行 | **并行 agents** 处理独立任务 |
| 基于感觉的"即兴编程" | **规范驱动** 具备完整可追溯性 |
| 分支中隐藏进度 | GitHub 中的 **透明审计轨迹** |
| 手动任务协调 | 使用 `/pm:next` 的**智能优先级排序** |

## 为什么使用 GitHub Issues？

大多数 Claude Code 工作流都是孤立运行的——单个开发者在本地环境中与 AI 协作。这产生了一个根本性问题：**AI 辅助开发成为了孤岛**。

通过使用 GitHub Issues 作为我们的数据库，我们释放了强大的功能：

### 🤝 **真正的团队协作**
- 多个 Claude 实例可以同时在同一项目上工作
- 人类开发者通过 issue 评论实时看到 AI 进度
- 团队成员可以随时加入——上下文始终可见
- 管理者无需打断工作流程就能获得透明度

### 🔄 **无缝的人机交接**
- AI 可以开始任务，人类可以完成（或反之）
- 进度更新对所有人可见，而不被困在聊天记录中
- 代码审查通过 PR 评论自然发生
- 没有"AI 做了什么？"的会议

### 📈 **可扩展到单独工作之外**
- 添加团队成员无摩擦入职
- 多个 AI agents 在不同 issues 上并行工作
- 分布式团队自动保持同步
- 与现有 GitHub 工作流和工具配合

### 🎯 **单一真相来源**
- 无需单独的数据库或项目管理工具
- Issue 状态就是项目状态
- 评论是审计轨迹
- Labels 提供组织结构

这不仅仅是一个项目管理系统——它是一个**协作协议**，让人类和 AI agents 能够大规模协作，使用您的团队已经信任的基础设施。

## 核心原则：杜绝即兴编程

> **每一行代码都必须能够追溯到规范。**

我们遵循严格的5阶段纪律：

1. **🧠 头脑风暴** - 思考得比舒适范围更深入
2. **📝 文档记录** - 编写不容误解的规范
3. **📐 规划** - 用明确的技术决策进行架构设计
4. **⚡ 执行** - 完全按照规范构建
5. **📊 跟踪** - 在每个步骤保持透明的进度

没有捷径。没有假设。没有遗憾。

## 系统架构

```
.claude/
├── CLAUDE.md          # 始终激活的指令（复制内容到你项目的 CLAUDE.md 文件中）
├── agents/            # 面向任务的 agents（用于上下文保持）
├── commands/          # 命令定义
│   ├── context/       # 创建、更新和预备上下文
│   ├── pm/            # ← 项目管理命令（本系统）
│   └── testing/       # 预备和执行测试（编辑这个）
├── context/           # 项目级别的上下文文件
├── epics/             # ← PM的本地工作区（放在 .gitignore 中）
│   └── [epic-name]/   # Epic 和相关任务
│       ├── epic.md    # 实现计划
│       ├── [#].md     # 单个任务文件
│       └── updates/   # 进行中的更新
├── prds/              # ← PM的 PRD 文件
├── rules/             # 放置你想引用的任何规则文件
└── scripts/           # 放置你想使用的任何脚本文件
```

## 工作流阶段

### 1. 产品规划阶段

```bash
/pm:prd-new feature-name
```
启动全面的头脑风暴，创建一个捕获愿景、用户故事、成功标准和约束条件的产品需求文档。

**输出：** `.claude/prds/feature-name.md`

### 2. 实现规划阶段

```bash
/pm:prd-parse feature-name
```
将 PRD 转换为带有架构决策、技术方法和依赖映射的技术实现计划。

**输出：** `.claude/epics/feature-name/epic.md`

### 3. 任务分解阶段

```bash
/pm:epic-decompose feature-name
```
将 epic 分解为具体的、可操作的任务，包含验收标准、工作量估算和并行化标记。

**输出：** `.claude/epics/feature-name/[task].md`

### 4. GitHub 同步

```bash
/pm:epic-sync feature-name
# 或者对于有把握的工作流：
/pm:epic-oneshot feature-name
```
将 epic 和任务推送到 GitHub，作为带有适当标签和关系的 issues。

### 5. 执行阶段

```bash
/pm:issue-start 1234  # 启动专门的 agent
/pm:issue-sync 1234   # 推送进度更新
/pm:next             # 获取下一个优先任务
```
专门的 agents 实现任务，同时保持进度更新和审计轨迹。

## 命令参考

> [!TIP]
> 输入 `/pm:help` 获取简洁的命令摘要

### 初始设置
- `/pm:init` - 安装依赖并配置 GitHub

### PRD 命令
- `/pm:prd-new` - 为新产品需求启动头脑风暴
- `/pm:prd-parse` - 将 PRD 转换为实现 epic
- `/pm:prd-list` - 列出所有 PRD
- `/pm:prd-edit` - 编辑现有 PRD
- `/pm:prd-status` - 显示 PRD 实现状态

### Epic 命令
- `/pm:epic-decompose` - 将 epic 分解为任务文件
- `/pm:epic-sync` - 将 epic 和任务推送到 GitHub
- `/pm:epic-oneshot` - 在一个命令中分解并同步
- `/pm:epic-list` - 列出所有 epics
- `/pm:epic-show` - 显示 epic 及其任务
- `/pm:epic-close` - 标记 epic 为完成
- `/pm:epic-edit` - 编辑 epic 详情
- `/pm:epic-refresh` - 从任务更新 epic 进度

### Issue 命令
- `/pm:issue-show` - 显示 issue 和子 issues
- `/pm:issue-status` - 检查 issue 状态
- `/pm:issue-start` - 用专门的 agent 开始工作
- `/pm:issue-sync` - 推送更新到 GitHub
- `/pm:issue-close` - 标记 issue 为完成
- `/pm:issue-reopen` - 重新打开已关闭的 issue
- `/pm:issue-edit` - 编辑 issue 详情

### 工作流命令
- `/pm:next` - 显示带有 epic 上下文的下一个优先 issue
- `/pm:status` - 整体项目仪表板
- `/pm:standup` - 每日站会报告
- `/pm:blocked` - 显示被阻塞的任务
- `/pm:in-progress` - 列出进行中的工作

### 同步命令
- `/pm:sync` - 与 GitHub 的完整双向同步
- `/pm:import` - 导入现有的 GitHub issues

### 维护命令
- `/pm:validate` - 检查系统完整性
- `/pm:clean` - 归档已完成的工作
- `/pm:search` - 跨所有内容搜索

## 并行执行系统

### Issues 不是原子的

传统思维：一个 issue = 一个开发者 = 一个任务

**现实：一个 issue = 多个并行工作流**

单个"实现用户认证"的 issue 不是一个任务。它是...

- **Agent 1**：数据库表和迁移
- **Agent 2**：服务层和业务逻辑
- **Agent 3**：API 端点和中间件
- **Agent 4**：UI 组件和表单
- **Agent 5**：测试套件和文档

所有这些都在同一个工作树中**同时**运行。

### 速度的数学

**传统方法：**
- 包含 3 个 issues 的 Epic
- 顺序执行

**本系统：**
- 同样包含 3 个 issues 的 Epic
- 每个 issue 分解为约4个并行流
- **12个 agents 同时工作**

我们不是将 agents 分配给 issues。我们是**利用多个 agents** 来更快交付。

### 上下文优化

**传统单线程方法：**
- 主对话承载所有实现细节
- 上下文窗口被数据库模式、API 代码、UI 组件填满
- 最终触及上下文限制并失去连贯性

**并行 agent 方法：**
- 主线程保持清洁和战略性
- 每个 agent 在隔离中处理自己的上下文
- 实现细节从不污染主对话
- 主线程保持监督而不淹没在代码中

你的主对话成为指挥家，而不是乐团。

### GitHub vs 本地：完美分离

**GitHub 看到的：**
- 清洁、简单的 issues
- 进度更新
- 完成状态

**本地实际发生的：**
- Issue #1234 爆发为5个并行 agents
- Agents 通过 Git 提交协调
- 复杂的协调过程隐藏在视野之外

GitHub 不需要知道工作是如何完成的——只需要知道它已经完成。

### 命令流

```bash
# 分析什么可以并行化
/pm:issue-analyze 1234

# 启动集群
/pm:epic-start memory-system

# 观看魔法
# 12个 agents 跨3个 issues 工作
# 全部在：../epic-memory-system/

# 完成时一次干净的合并
/pm:epic-merge memory-system
```

## 关键特性与优势

### 🧠 **上下文保持**
再也不会丢失项目状态。每个 epic 维护自己的上下文，agents 从 `.claude/context/` 读取，并在同步前本地更新。

### ⚡ **并行执行**
通过多个 agents 同时工作更快交付。标记为 `parallel: true` 的任务启用无冲突的并发开发。

### 🔗 **GitHub 原生**
与你的团队已经使用的工具配合工作。Issues 是真相的来源，评论提供历史，不依赖于 Projects API。

### 🤖 **Agent 专业化**
每个工作都有合适的工具。UI、API 和数据库工作使用不同的 agents。每个都自动读取需求并发布更新。

### 📊 **完全可追溯性**
每个决策都被记录。PRD → Epic → Task → Issue → Code → Commit。从想法到生产的完整审计轨迹。

### 🚀 **开发者生产力**
专注于构建，而不是管理。智能优先级排序、自动上下文加载，以及准备好时的增量同步。

## 经过验证的效果

使用这个系统的团队报告：
- **89% 更少时间** 在上下文切换上的损失——你会少使用很多 `/compact` 和 `/clear` 
- **5-8个并行任务** vs 之前的1个——同时编辑/测试多个文件
- **75% 减少** bug 率——由于将功能分解为详细任务
- **高达3倍更快** 功能交付——基于功能大小和复杂性

## 示例流程

```bash
# 开始新功能
/pm:prd-new memory-system

# 审查和完善 PRD...

# 创建实现计划
/pm:prd-parse memory-system

# 审查 epic...

# 分解为任务并推送到 GitHub
/pm:epic-oneshot memory-system
# 创建 issues：#1234（epic），#1235，#1236（任务）

# 开始在任务上开发
/pm:issue-start 1235
# Agent 开始工作，维护本地进度

# 同步进度到 GitHub
/pm:issue-sync 1235
# 更新作为 issue 评论发布

# 检查整体状态
/pm:epic-show memory-system
```

## 立即开始

### 快速设置（2分钟）

1. **将此仓库克隆到你的项目中**：
   ```bash
   cd path/to/your/project/
   git clone https://github.com/automazeio/ccpm.git .
   ```
   > ⚠️ **重要**：如果你已经有 `.claude` 目录，将此仓库克隆到不同目录，然后将克隆的 `.claude` 目录内容复制到你项目的 `.claude` 目录。

2. **初始化 PM 系统**：
   ```bash
   /pm:init
   ```
   此命令将：
   - 安装 GitHub CLI（如果需要）
   - 使用 GitHub 进行身份验证
   - 安装 [gh-sub-issue extension](https://github.com/yahsan2/gh-sub-issue) 以实现正确的父子关系
   - 创建必需的目录
   - 更新 .gitignore

3. **创建包含你仓库信息的 `CLAUDE.md`**：
   ```bash
   /init include rules from .claude/CLAUDE.md
   ```
   > 如果你已经有 `CLAUDE.md` 文件，运行：`/re-init` 来用 `.claude/CLAUDE.md` 中的重要规则更新它。

4. **预备系统**：
   ```bash
   /context:create
   ```



### 开始你的第一个功能

```bash
/pm:prd-new your-feature-name
```

观看结构化规划如何转变为已交付的代码。

## 本地 vs 远程

| 操作 | 本地 | GitHub |
|-----------|-------|--------|
| PRD 创建 | ✅ | — |
| 实现规划 | ✅ | — |
| 任务分解 | ✅ | ✅ (同步) |
| 执行 | ✅ | — |
| 状态更新 | ✅ | ✅ (同步) |
| 最终交付物 | — | ✅ |

## 技术说明

### GitHub 集成
- 使用 **gh-sub-issue extension** 实现正确的父子关系
- 如果扩展未安装则回退到任务列表
- Epic issues 自动跟踪子任务完成情况
- Labels 提供额外的组织结构（`epic:feature`，`task:feature`）

### 文件命名约定
- 任务在分解期间从 `001.md`，`002.md` 开始
- GitHub 同步后，重命名为 `{issue-id}.md`（例如，`1234.md`）
- 使导航变得容易：issue #1234 = 文件 `1234.md`

### 设计决策
- 有意避免 GitHub Projects API 复杂性
- 所有命令首先在本地文件上操作以提高速度
- 与 GitHub 的同步是明确的和受控的
- Worktrees 为并行工作提供清洁的 git 隔离
- GitHub Projects 可以单独添加用于可视化

---

## 支持这个项目

Claude Code PM 在 [Automaze](https://automaze.io) 开发，**为交付的开发者，由交付的开发者打造**。

如果 Claude Code PM 帮助你的团队交付更好的软件：

- ⭐ **[为这个仓库点星](https://github.com/your-username/claude-code-pm)** 表达你的支持
- 🐦 **[在 X 上关注 @aroussi](https://x.com/aroussi)** 获取更新和技巧


---

> [!TIP]
> **使用 Automaze 更快交付。** 我们与创始人合作，将他们的愿景变为现实，扩展他们的业务，并优化成功。
> **[访问 Automaze 与我预约通话 ›](https://automaze.io)**
