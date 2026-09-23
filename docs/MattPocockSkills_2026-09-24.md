# Matt Pocock Skills

> 收录时间：2026-09-24 | 原始链接：https://github.com/mattpocock/skills | ⭐ 268,460 | 🍴 22,634

## 基础信息

| 项 | 内容 |
|----|------|
| 作者 | Matt Pocock（mattpocock） |
| 开源协议 | MIT |
| 技术栈 | Shell（主）、JavaScript |
| 所属领域 | AI工程实践、代码开发工具 |
| 核心用途 | AI研发流程改造 |
| 最近更新 | 2026-09-18 |
| 创建时间 | 2026-02-03 |
| 最新版本 | v1.2.3（Changesets 管理） |
| 官网 | https://aihero.dev/skills |
| 仓库规模 | 169 个文件，38 个技能 |
| Open Issues | 523 |

## 项目简介

Matt Pocock（TypeScript 圈知名教育者，Total TypeScript / AI Hero 创始人）日常使用的 Agent 技能集，副标题即定位宣言：

> **"Skills for Real Engineers"** — my agent skills that I use every day to do real engineering, **not vibe coding**.

**它最鲜明的态度是反「流程接管」**。README 开篇直接点名批评同类方案：

> Approaches like **GSD, BMAD, and Spec-Kit** try to help by **owning the process**. But while doing so, they **take away your control** and make bugs in the process hard to resolve.

它的方案反过来：**"small, easy to adapt, and composable"**——「Hack around with them. **Make them your own.**」并强调 **"They work with any model"**。

理论血统来自经典工程著作而非自创：*The Pragmatic Programmer*、Eric Evans《领域驱动设计》、Ousterhout《软件设计哲学》、Kent Beck《解析极限编程》。

## 它针对的四类失败模式

| # | 失败模式 | 解法 | 对应技能 |
|---|---|---|---|
| 1 | **Agent 没做我想要的**（对齐失败） | **grilling 拷问式访谈**——让 Agent 反复追问，直到设计树每个分支都被解决 | `/grill-me`、`/grill-with-docs` |
| 2 | **Agent 太啰嗦** | **`CONTEXT.md` 共享语言**——用项目术语把「20 个词」压成「1 个词」 | 内建于 `/grill-with-docs` |
| 3 | **代码跑不起来** | 反馈回路：静态类型 + 浏览器 + TDD red-green-refactor；调试走纪律化环路 | `/tdd`、`/diagnosing-bugs` |
| 4 | **代码变泥球** | 关心代码设计——深模块（deep modules），定期扫描架构 | `/improve-codebase-architecture`、`/codebase-design` |

### 核心机制一：grilling（设计树 + 轮次）

不是「问几个问题」，而是把方案建模成**设计树**（design tree）——每个决策分叉出挂在它下面的决策。分**轮次**推进：

- **frontier（前沿）** = 所有前置条件已settle的决策，即**现在就能问、不用猜**的那些
- 一轮问整个 frontier，每题**编号 + 给出推荐答案**
- 用户答完后，已settle的决策把 frontier 向外推，解锁依赖它们的问题 → 重算 frontier 问下一轮
- 依赖本轮其他未决问题的题目，属于**后续轮次**
- **找事实是 Agent 的活，不是用户的**——需要环境事实就派子 Agent 去查，不阻塞其余问题
- **决策是用户的**——每个都问、然后等

> 终止条件：frontier 为空 = 设计树每个分支都访问过、没有任何静默假设。**用户确认达成共识前不得动手实施。**

### 核心机制二：`CONTEXT.md` 共享语言

作者自称「**可能是本仓库最酷的技术**」。举例（来自其 `course-video-manager` 仓库）：

- **BEFORE**：「课程某章节里的一节被『实体化』（即给了文件系统位置）时有问题」
- **AFTER**：「**实体化级联**（materialization cascade）有问题」

README 列出的连带收益：

- 变量、函数、文件命名一致，均使用共享语言
- 代码库对 Agent **更好导航**
- Agent **思考更省 token**（因为语言更简洁）

## 技能总览：38 个（25 稳定 + 9 in-progress + 4 misc）

### 最独特的机制：按「谁能调用」二分

> **User-invoked** —— 只有你敲才触发（如 `/grill-me`），职责是**编排**
> **Model-invoked** —— 你也行、Agent 也能按任务自动触发，承载**可复用纪律**
> 规则：user-invoked 可以调用 model-invoked，但**绝不调用另一个 user-invoked**

### Engineering（18）

**User-invoked（编排层）**

| 技能 | 作用 |
|---|---|
| `ask-matt` | **路由器**——问「我这种情况该用哪个技能或流程」 |
| `grill-with-docs` | grilling + 同时构建项目领域模型、锐化术语、就地更新 `CONTEXT.md` 与 ADR |
| `triage` | 让 issue 在分诊角色状态机中流转 |
| `improve-codebase-architecture` | 扫描代码库深化机会 → 出**可视化 HTML 报告** → 挑一个接着 grill |
| `setup-matt-pocock-skills` | 每仓库跑一次的初始化（issue tracker、分诊标签、文档布局） |
| `to-spec` | 把当前对话合成规格并发布到 issue tracker（**不访谈**，只综合已讨论内容） |
| `to-tickets` | 把计划/规格/对话拆成一组 **tracer-bullet 工单**，各自声明阻塞边，可写入本地文件或真实 tracker 的阻塞链 |
| `implement` | 按规格或工单施工，在**预先商定的接缝**处驱动 `/tdd`，提交前走 `/code-review` |
| `wayfinder` | 规划**单个 Agent 会话装不下**的大块工作，做成 issue tracker 上的决策工单地图，逐个解决 |

**Model-invoked（纪律层）**：`prototype`、`diagnosing-bugs`、`research`、`tdd`、`domain-modeling`、`codebase-design`、`code-review`、`resolving-merge-conflicts`、`wizard`

几个值得注意的：

- **`code-review`**：**双轴**审查 diff（相对固定基点）——**Standards**（是否符合仓库编码规范 + Fowler 坏味道基线）与 **Spec**（是否忠实地实现了原始 issue/规格），**两轴跑并行子 Agent 以免互相污染**
- **`diagnosing-bugs`**：纪律化诊断环——**建一个「会因这个 bug 变红」的反馈回路** → 最小化 → 假设 → 插桩 → 修复 → 回归测试，分阶段门控
- **`resolving-merge-conflicts`**：逐 hunk 处理进行中的 merge/rebase 冲突，按意图追溯到两侧的一手来源，然后完成操作（**绝不 `--abort`**）
- **`wizard`**：生成交互式 bash 向导，带人走**只有人能做的步骤**（开云资源、配凭据或 CI secret、走陌生第三方控制台、一次性迁移/切换）
- **`codebase-design`**：深模块（大量行为藏在简单接口后、放在干净接缝处、可通过该接口测试）的共享纪律与词汇

### Productivity（7）

**User-invoked**：`grill-me`（**最受欢迎**）、`handoff`（把对话压缩成交接文档给下一个 Agent）、`teach`（多会话教学，当前目录作有状态教学工作区）、`to-questionnaire`（把**你答不了**的决策变成给能答的人填的问卷，grill 的是「怎么发」而非主题）、`wait-what`（消息没看懂就敲它，Agent 用你 `CONTEXT.md` 的词汇重讲一遍）

**Model-invoked**：`grilling`（可复用的访谈原语，被 `grill-me`/`grill-with-docs`/`triage`/`wayfinder`/`improve-codebase-architecture` 共用）、`writing-for-agents`（教怎么写技能、`AGENTS.md`/`CLAUDE.md` 等 Agent 可达文档）

### Misc（4）与 In-progress（9）

- **misc**：`git-guardrails-claude-code`、`migrate-to-shoehorn`、`scaffold-exercises`、`setup-pre-commit`
- **in-progress**：`claude-handoff`、`implement-spec`、`loop-me`、`pr`、`retro`、`setup-ts-deep-modules`、`writing-beats`、`writing-fragments`、`writing-shape`

## 安装

**两种哲学，二选一**（README 明确警告：都装会导致每个技能出现两次）

```bash
# ① Claude Code 插件 —— 托管只读包，随作者更新（「订阅」而非 fork）
claude plugins install mattpocock-skills
# 或在会话内：
/plugin install mattpocock-skills

# ② skills.sh —— 把可编辑技能文件拷进你的项目（可改可拆，你的领地）
npx skills@latest add mattpocock/skills
```

用 `skills.sh` 安装时**务必勾选 `setup-matt-pocock-skills`**，然后每仓库跑一次 `/setup-matt-pocock-skills`，它会问你：用哪个 issue tracker（GitHub / Linear / 本地文件）、分诊用哪些标签、文档存哪里。

`npx skills update` 可在需要时拉取作者最新改动——**不会背着你自己更新**。

## ⭐ 与 superpowers 的区别（本库已收录 obra/superpowers）

| 维度 | superpowers | Matt Pocock Skills |
|---|---|---|
| **星数** | 288,562 | 268,460 |
| 创建时间 | 2025-10-09 | 2026-02-03 |
| 许可 | MIT | MIT |
| **技能数** | 15 | **38**（25 稳定） |
| **最大单文件** | 32.6 KB | 11.9 KB |
| **核心主张** | **"Mandatory workflows, not suggestions"**（强制工作流） | **小、可组合、你自己拆改** |
| **调用方式** | **按上下文自动触发**，首轮注入 bootstrap | **显式 `user-invoked` / `model-invoked` 二分** |
| **控制权** | Agent 接管流程，可自主连续工作**数小时** | 你显式调用编排器，**每个决策都是你的** |
| **对同类方案态度** | 自己即「接管流程」学派 | **明确批评**接管流程（点名 GSD/BMAD/Spec-Kit） |
| **技能重心** | 绿地全流程：brainstorm→worktree→计划→实施→TDD→复审→收尾 | 兼有大量**存量代码库治理**工具 |
| **独门机制** | 子 Agent 两阶段复审、git worktree 隔离 | **grilling 设计树**、**`CONTEXT.md` 共享语言**、`wizard` |
| **啰嗦问题** | 无专门机制 | **`CONTEXT.md` 共享语言**（作者称最酷技术） |
| **理论来源** | 自建四条哲学（TDD优先/系统化/复杂度削减/证据优于声明） | 明确引用 Pragmatic Programmer、DDD、软件设计哲学、XP |

**一句话总结差异**：superpowers 是**流水线**（你上车，它开）；Matt Pocock Skills 是**工具箱**（你开车，它导航）。

**两者不冲突，已被第三方融合**：`AppleCG/devflow` 把 **superpowers + mattpocock/skills + OpenSpec** 融为一条流程——Design（grill → spec）、Build（8 阶段 TDD 流水线）、Fix（diagnose → rollback → verify）。

## 生态观察：中文化异常活跃

| 项目 | ★ | 说明 |
|---|---|---|
| `vinvcn/mattpocock-skills-zh-CN` | **4,416** | 简体中文本地化版 |
| `devcxl/mattpocock-skills-zh` | 386 | 中文翻译版，**每日中午 12 点同步** |
| `tt-a1i/matt-skills-with-to-goal` | 173 | 基于 v1.1，Planning → 可验证目标 → 新会话实施 |
| `FeatherHunter/dsh-mattpocock-skills-deck` | 79 | DeepSeek Harness 插件，自带 v1.2.3 全部 25 技能 |
| `bbylw/mattpocock-skills-cn` | 38 | 中文技能包 |
| `CNife/learn-mattpocock-skills` | 18 | 中英双语学习图谱 |
| `gongyijie85/mattpocock-skills-dsh` / `xiaoxiaosrm/dsh-mattpocock-skills` | 15 / 8 | DSH 移植版 |
| `Boom-Vitt/skills-thai` | 5 | 泰语翻译版 |

另有 `mattpocock/dictionary-of-ai-coding`（★4,741，AI 编码术语白话词典）与 `mattpocock/sandcastle`（★8,118，TypeScript 沙箱化编码 Agent 编排）同属作者作品。

## 优势与特点

- **不接管流程**——技能小、可读、可改，装了就是你的文件；`npx skills update` 才更新，不背着你动
- **模型无关**——README 明示 "work with any model"；CHANGELOG v1.2.3 专门有一条：把 `code-review`/`codebase-design` 里的 Claude Code 工具名去掉，「so the step is followable on Codex and other harnesses」
- **显式调用层级**——user-invoked 编排 / model-invoked 纪律，规则清晰（前者可调后者，不可同级互调）
- **经典工程理论支撑**——不是拍脑袋的提示词合集
- **存量代码库治理**——`improve-codebase-architecture` 建议每隔几天跑一次
- **活体维护**——v1.2.3 仍在迭代（Changesets 管理版本，PR 流程规范）

## 局限与注意事项

- **需宿主 Agent**——技能是 Markdown，靠 Claude Code / Codex / Cursor 等执行；无 Agent 则无用
- **以英文为主**——原文技能说明为英文（有多个第三方汉化版可选）
- **部分技能依赖 issue tracker 工作流**——`to-spec` / `to-tickets` / `triage` / `wayfinder` 假定你有 GitHub / Linear / 本地文件工单体系；不用 tracker 的团队这几个技能价值打折
- **`in-progress` 目录不稳定**——9 个技能标注进行中，接口可能变
- **`deprecated` 目录为空**——说明作者会主动淘汰技能，但当前无废弃项
- **523 个开放 issue**——热度高但积压也不少
- **未经信创环境验证**——未在麒麟/ARM64 等国产化环境实测

## 适用性判断

### ✅ 对 VTF 场景直接对口

| 技能 | 为什么适合 |
|---|---|
| **`domain-modeling` + `CONTEXT.md`** | 你的痛点之一是「申报业务类别 ↔ 申报/回报报文字段对应关系」——**本质是领域术语密集**问题。把「20 个词压成 1 个词」的机制，对 Agent 理解 VTF 业务域的效率提升很直接 |
| **`improve-codebase-architecture`** | 扫描存量代码库出深化机会，产物是**可视化 HTML 报告**——与你偏好单文件 HTML + 侧边目录的形态一致 |
| **`code-review` 双轴** | Standards（编码规范 + 坏味道）与 Spec（是否忠实实现原始需求）分轴并行——对**合规留痕**场景，两轴分离比混在一起更可追溯 |
| **`diagnosing-bugs`** | 分阶段门控 + 「建会因 bug 变红的反馈回路」，适合你「诊断透彻后再修复」的要求 |

### ⚠️ 需注意

- **第 2 条失败模式（Agent 太啰嗦）对你的 ollama 浪费问题有启发**——`deepseek-v4.1-flash-ollama` 单次 13~79 万 token 的浪费，部分可能正是「用 20 个词说 1 个词的事」。`CONTEXT.md` 共享语言是低成本切入点
- **`grilling` 是交互式机制**——需要你投入时间回答问题；纯自动化流水线场景用不上
- **不替代 superpowers**——两者可并用，已有第三方融合先例

### 🔴 不建议

- 把它作为**客户交付物**（无公司实体背书、`in-progress` 目录不稳定）
- 在**没有 issue tracker 工作流**的团队里全量引入（`to-spec`/`triage`/`wayfinder` 会空转）

## 相关资源

- 仓库：https://github.com/mattpocock/skills
- 官网 / 新闻订阅（约 6 万开发者）：https://aihero.dev/skills
- skills.sh 页：https://skills.sh/mattpocock/skills
- 作者其他作品：[dictionary-of-ai-coding](https://github.com/mattpocock/dictionary-of-ai-coding)（★4,741）、[sandcastle](https://github.com/mattpocock/sandcastle)（★8,118）、[evalite](https://github.com/mattpocock/evalite)（★1,692）
- 汉化版：[英文原版](https://github.com/mattpocock/skills) · [zh-CN ★4,416](https://github.com/vinvcn/mattpocock-skills-zh-CN)
- 对照收录：[superpowers](https://github.com/obra/superpowers)（本库序号 10）

## 标签

`#Agent Skill` `#AI研发流程` `#软件工程` `#TDD` `#代码评审` `#架构治理` `#领域建模` `#共享语言` `#grilling` `#深模块` `#MIT` `#TypeScript社区`
