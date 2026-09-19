# superpowers

> 收录时间：2026-09-19 | 原始链接：https://github.com/obra/superpowers | ⭐ 288,562 | 🍴 25,808

## 基础信息

| 项 | 内容 |
|----|------|
| 作者 | obra（Jesse Vincent） |
| 开源协议 | MIT |
| 技术栈 | Shell（246.7 KB）、JavaScript（184.8 KB）、Python（20.2 KB）、TypeScript、HTML、Batchfile |
| 所属领域 | AI工程实践、代码开发工具 |
| 核心用途 | AI研发流程改造 |
| 最近更新 | 2026-09-19 |
| 创建时间 | 2025-10-09 |
| 最新版本 | v6.4.1（2026-09-19 发布） |
| 仓库规模 | 231 个文件、15 个技能、5.1 MB |
| Open Issues | 370 |

## 项目简介

superpowers 是给**编码 Agent** 装的一套**软件开发方法论**——不是工具，也不是提示词合集，而是把「先澄清需求、再写方案、按 TDD 实施、每步自审」这套工程纪律固化成 15 个可组合技能，并保证 Agent 会真的执行。

它的定位在 README 里说得很直白：*"An agentic skills framework & software development methodology that works."*（一个**能真正生效**的技能框架与开发方法论）。

**它解决的核心问题是：Agent 会写代码，但不会做工程。** 默认状态下，编码 Agent 一看到需求就急着写实现，跳过需求澄清、跳过方案评审、跳过测试。superpowers 的做法是在 Agent 首轮注入一段 bootstrap，声明「Mandatory workflows, not suggestions」——**这些流程是强制工作流，不是可选建议**，技能按上下文自动触发，不需要用户主动调用。

## 核心功能

### 15 个技能（按类别）

| 类别 | 技能 | 作用 |
|------|------|------|
| **测试** | test-driven-development | RED-GREEN-REFACTOR 循环，含测试反模式参考 |
| **调试** | systematic-debugging | 四阶段根因定位（含根因追溯、纵深防御、基于条件的等待） |
| | verification-before-completion | 确认问题**真的**修好了，而非声称修好了 |
| | diagnosing-superpowers | 会话失败取证：导出脱敏诊断包或提交 issue |
| **协作** | brainstorming | 苏格拉底式设计打磨 |
| | writing-plans | 详细实施计划 |
| | executing-plans | 内联计划执行：单上下文、结尾一次总审 |
| | dispatching-parallel-agents | 并发子 Agent 工作流 |
| | requesting-code-review | 复审前检查清单 |
| | receiving-code-review | 如何回应评审意见 |
| | using-git-worktrees | 并行开发分支 |
| | finishing-a-development-branch | 合并 / PR 决策工作流 |
| | subagent-driven-development | 子 Agent 快速迭代 + 两阶段复审 |
| **元技能** | writing-skills | 按最佳实践创建新技能（含测试方法论） |
| | using-superpowers | 技能体系导引 |

技能体量差异很大：`subagent-driven-development` 是最大的（**32.6 KB**），其次是 `writing-skills`（26.6 KB）、`executing-plans`（20.4 KB）、`brainstorming`（17.5 KB）；最小的 `requesting-code-review` 仅 3.0 KB。

### 七步强制工作流

| 步 | 技能 | 触发时机与产物 |
|----|------|----------------|
| 1 | **brainstorming** | 写代码**之前**。通过提问打磨粗糙想法、探索备选方案、分块展示设计待确认，保存设计文档 |
| 2 | **using-git-worktrees** | 设计通过**之后**。在新分支建隔离工作区、跑项目初始化、验证测试基线干净 |
| 3 | **writing-plans** | 有了获批设计。把工作拆成 **2–5 分钟一个**的小任务，每个任务含精确文件路径、完整代码、验证步骤 |
| 4 | **subagent-driven-development** 或 **executing-plans** | 有了计划。前者为每个任务派新子 Agent 并在每任务后复审（最彻底）；后者在当前会话内联实施、结尾对整条分支做一次新复审（最省） |
| 5 | **test-driven-development** | 实施期间。强制先写失败测试 → 看它失败 → 写最小实现 → 看它通过 → 提交。**删除先于测试写下的代码** |
| 6 | **requesting-code-review** | 任务之间。对照计划复审，按严重度报告问题，**关键问题阻断进度** |
| 7 | **finishing-a-development-branch** | 任务完成。验证测试、给出合并/PR/保留/丢弃选项、清理 worktree |

README 强调：*"The agent checks for relevant skills before any task. Mandatory workflows, not suggestions."* 据其描述，Agent 常能**连续自主工作数小时**而不偏离既定计划。

### 四条设计哲学

- **Test-Driven Development** —— 永远先写测试
- **Systematic over ad-hoc** —— 流程优于猜测
- **Complexity reduction** —— 简洁是首要目标
- **Evidence over claims** —— **宣布成功之前先验证**

最后一条与本文档库一贯的「数据可溯源、不臆造」原则同源。

## 兼容矩阵

已适配 **16 种编码 Agent / harness**，每种独立安装：

Claude Code、Codex App、Codex CLI、Cursor、Antigravity、Devin CLI、Factory Droid、Gemini CLI、GitHub Copilot CLI、Grok Build CLI、Kimi Code、OpenCode、Pi、Qwen Code、**Hermes Agent**、Muse

仓库为每种 harness 提供独立适配层：`.claude-plugin`、`.codex-plugin`、`.cursor-plugin`、`.devin-plugin`、`.hermes-plugin`、`.kimi-plugin`、`.muse-plugin`、`.opencode`、`.pi`。

## 安装与使用

### Hermes Agent

```bash
hermes plugins install obra/superpowers --enable
```

安装后需重启活跃的 Hermes 会话。

**⚠️ Hermes 专属已知限制**（README 原文）：*"Hermes has no post-compaction hook, so a very long session that compacts over its first turn loses the bootstrap — start a fresh session if skills stop triggering."*
即：Hermes 没有 post-compaction 钩子，超长会话一旦在首轮之后发生上下文压缩，bootstrap 会丢失，**需重开会话**才能恢复技能触发。

### 其他 harness（摘录）

```bash
# Claude Code（Anthropic 官方插件市场）
/plugin install superpowers@claude-plugins-official

# Antigravity
agy plugin install https://github.com/obra/superpowers

# Codex CLI
/plugins            # 搜索 superpowers → Install Plugin
```

## Hermes 适配实现细节

对 Hermes 的适配在一个不到 4 KB 的插件里完成（`.hermes-plugin/__init__.py`，3,937 B + `plugin.yaml`），值得作为**「如何把技能体系移植到 Hermes」的参考实现**：

- 向 Hermes 注册钩子 **`pre_llm_call`**，在**首轮**把 bootstrap 内容注入用户消息（`{"context": ...}`）。
- Bootstrap 用 `<EXTREMELY_IMPORTANT>` 包裹，内含 `using-superpowers` 技能正文 + 一份 `references/hermes-tools.md` **工具名映射表**（把技能里写的 Claude Code 工具名映射到 Hermes 工具名）。
- 把 `skills/` 下每个技能用 `ctx.register_skill()` 注册进 Hermes 原生技能加载器，可用 `skill_view("superpowers:brainstorming")` 这种**命名空间形式**调用。
- 代码里留了两条**踩坑注释**（均标注「verified 2026-07-23」）：①`register_skill` 必须传 `pathlib.Path`，传 `str` 会抛 `AttributeError` 且 Hermes **静默禁用整个插件**；②`on_session_start` 的返回值会被忽略、`ctx.inject_message` 在该钩子里会被拒，因此只能走 `pre_llm_call`。
- 定位 `skills/` 目录时若两种布局都不匹配，会**显式抛错**而非静默跳过——注释解释：*"a bootstrap that silently skips is how a broken install masquerades as a working one."*

## 与本地已有技能的关系

**本机 Hermes 技能库中已内置 3 个改编自本项目的技能**，frontmatter 明确标注 `author: Hermes Agent (adapted from obra/superpowers)`：

| 技能 | 本地版本 | 说明 |
|------|----------|------|
| `test-driven-development` | v1.1.0 | TDD：强制 RED-GREEN-REFACTOR，先测试后代码 |
| `systematic-debugging` | v1.1.0 | 四阶段根因调试 |
| `requesting-code-review` | v2.0.0 | 提交前复审：安全扫描、质量闸门、自动修复（另标注 *adapted from obra/superpowers + MorAlekss*） |

**尚未内置的 12 个**：brainstorming、diagnosing-superpowers、dispatching-parallel-agents、executing-plans、finishing-a-development-branch、receiving-code-review、subagent-driven-development、using-git-worktrees、using-superpowers、verification-before-completion、writing-plans、writing-skills。其中 Hermes 用自有的 `plan`、`hermes-agent-skill-authoring` 覆盖了后两个的职能。

**注意重名冲突**：若通过 `hermes plugins install` 整体安装，会与上述 3 个同名的本地改编版并存。安装前需明确谁的优先级更高，或只挑缺失的技能引入。

## 使用场景

### ✅ 适用

- **平台代码开发**：C++ 撮合引擎、Java 后端、Python 脚本等「多文件、要测试」的工程任务
- **需要强测试保障的改造**：如金融报文解析、加密与协议改造——writing-plans 的细粒度任务拆解 + TDD 强制，可抑制「改一处崩三处」
- **长时段自主开发**：靠子 Agent 逐任务复审维持不偏离计划
- **并发子 Agent 编排**：`dispatching-parallel-agents` 与 `subagent-driven-development` 正是为多子 Agent 场景而写

### ❌ 不适用（避免硬套）

- **文档类工作流**：流程合规填报、整改台账、抽查反馈单、周报——15 个技能全部只为「写代码」设计，对填报、归档、跨部门协同零帮助
- **数据采集与报告生成**：周巡检、日报、知识库建设，与 TDD / 代码复审无交集
- **业务分析**：申报业务类别、报文字段映射等，需要业务知识库与差异比对，而非开发方法论
- **单次小改动 / 临时脚本**：七步流程（澄清→worktree→计划→子 Agent→TDD→复审→收尾）对二十行的活是纯负担
- **非 git 管理的工作**：第 2、7 步强依赖 git worktree

**一句话**：它是「**写代码时的纪律**」，不是「**干活的通用流程**」。

## 优势与特点

1. **强制性而非建议性** —— 核心差异在于「Mandatory workflows, not suggestions」，解决 Agent「知道该做但不做」的问题。
2. **流程粒度极细** —— 计划任务拆到 2–5 分钟，每项带精确文件路径与验证步骤，降低执行漂移。
3. **证据优于声明** —— `verification-before-completion` 专门防「声称修好了」，与证据驱动的工程文化一致。
4. **移植参考价值** —— `.hermes-plugin` 是「把外部技能体系接入 Hermes」的完整范例（钩子选型、工具名映射、技能注册、踩坑注释）。
5. **生态位明确** —— MIT 协议、288K stars、25.8K forks、持续活跃（收录当日仍发新版），已是该品类的事实标准。

## 注意事项

- **不是 Star/Fork 而是归档收录**：本条目是技术资源收录，不代表在本机安装或启用。
- **README 含商业服务链接**（`sales@primeradiant.com` 企业支持），评估时可将「项目质量」与「商业动机」分开看待。
- **Open Issues 370 个**，属活跃项目常态，但调研时建议先查 issue 是否涉及你关心的 harness。
- **社区中文版**：`jnMetaCode/superpowers-zh`（⭐8,148，MIT，14 个汉化技能 + 4 个中国原创技能）。**其 README 商业化程度高**——含赞助商 banner、带返利码的推广链接（`aff=` / `referral_code=`）、导流至课程站点；技能本身为 MIT 授权，但上游 `obra/superpowers` 才是权威源。

## 标签

`#AI工程实践` `#代码开发工具` `#AI研发流程改造` `#Agent技能框架` `#TDD` `#子Agent编排` `#开发方法论` `#Hermes插件`
