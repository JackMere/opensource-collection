# OpenCodeReview

> 收录时间：2026-09-21 | 原始链接：https://github.com/alibaba/open-code-review | ⭐ 38,353 | 🍴 2,734

## 基础信息

| 项 | 内容 |
|----|------|
| 作者 | Alibaba（阿里巴巴集团） |
| 开源协议 | Apache-2.0（Copyright 2026 Alibaba） |
| 技术栈 | Go（3.47 MB）、JavaScript、TypeScript、Kotlin、CSS、HTML |
| 所属领域 | 代码开发工具、AI工程实践 |
| 核心用途 | AI研发流程改造 |
| 最近更新 | 2026-09-20 |
| 创建时间 | 2026-05-18 |
| 官网 | https://open-codereview.ai |
| npm 包 | `@alibaba-group/open-code-review`（命令 `ocr`） |
| 文档 | https://open-codereview.ai/docs |
| 基准数据集 | [AACR-Bench](https://huggingface.co/datasets/Alibaba-Aone/aacr-bench)（Hugging Face） |
| 仓库规模 | 928 个文件、54.6 MB |
| Open Issues | 243 |
| 认证 | OpenSSF Best Practices **Gold** |

## 项目简介

阿里巴巴**内部使用两年**的官方 AI 代码评审助手，服务数万名开发者、累计发现数百万代码缺陷，经大规模验证后开源。

它读取 Git diff，通过具备工具调用能力的 Agent 把改动文件送给可配置的 LLM，产出**行级精确**的结构化评审意见。Agent 能读取完整文件内容、检索代码库、查看其他改动文件以补足上下文——因此给出的是深度评审而非表层 diff 评论。

**核心主张：确定性工程 × Agent 混合架构。** 把评审环节分成两类处理——**必须不能错**的交由工程逻辑（而非语言模型）保证，**需要动态判断**的才交给 Agent。

## 要解决的问题（通用 Agent 的三个痛点）

用 Claude Code 之类的通用 Agent 配 Skills 做代码评审时，作者总结出三个反复出现的问题：

| 痛点 | 表现 |
|---|---|
| **覆盖不全** | 改动集较大时，Agent 会「抄近路」——选择性只审部分文件，漏掉其余 |
| **定位漂移** | 报告的 issue 与实际代码位置对不上，行号或文件引用偏靶 |
| **质量不稳** | 自然语言驱动的 Skill 难以调试，提示词稍有差异评审质量就明显波动 |

**根因**：纯语言驱动的架构，**对评审过程缺少硬约束**。

## 核心设计：确定性工程 × Agent | 各司其职

### 确定性工程侧（硬约束）

| 机制 | 作用 |
|---|---|
| **精确文件选择** | 确定哪些文件需要评审、哪些应被过滤，确保关键改动不被遗漏 |
| **智能文件打包** | 把相关文件合并为一个评审单元（如 `message_en.properties` 与 `message_zh.properties` 打包同审）。**每个包作为隔离上下文的子 Agent 运行**——分治策略在超大改动集上依然稳定，且天然支持并发评审 |
| **细粒度规则匹配** | 按文件特征匹配评审规则，让模型注意力高度聚焦、从源头消除信息噪音。相比纯语言驱动的规则指导，**模板引擎**驱动的匹配更稳定可预测 |
| **外置的定位与反思模块** | 独立的评论定位模块与评论反思模块，系统性改善 AI 反馈的**位置准确度**与**内容准确度** |

### Agent 侧（动态决策）

- **场景调优的提示词** —— 面向代码评审深度优化的模板，在提升效果的同时降低 token 消耗
- **场景调优的工具集** —— 从大规模生产数据的**工具调用轨迹**中蒸馏而来（含调用频次分布、每工具的重复率、新增工具对整体调用链的影响），比通用 Agent 工具箱更稳定可预测

## 实测基准（AACR-Bench）

自建真实世界代码评审基准：**50** 个热门开源仓库、**200** 个真实 PR、**10** 种编程语言，由 **80+** 位资深工程师交叉校验，共 **1,505** 条标注真值。

作者给出的对比结论（**同一底层模型**，对比 Claude Code + Skills）：

| 指标 | 结论 |
|---|---|
| **Precision / F1** | 显著更高 |
| **token 消耗** | 仅约 **1/9** |
| **速度** | 更快 |
| **Recall** | **低于**通用 Agent——**刻意的取舍**（宁偏精确、少报噪音） |

> ⚠️ 注意这是**厂商自建基准上的自我评测**，方法论与数据集已公开（Hugging Face 可查），但选型时仍建议在自己的代码库上验证。

## 安装与使用

### 前置要求

- **Git >= 2.41** —— 依赖 Git 做 diff 生成、代码搜索与仓库操作

### 安装

```bash
npm install -g @alibaba-group/open-code-review
# 装后全局可用 `ocr` 命令
```

另提供安装脚本、GitHub Release 二进制、源码编译等方式（见官网 Installation）。

### 配置 LLM

```bash
ocr config provider          # 选内置 provider 或添加自定义
ocr config model             # 为当前 provider 选模型
```

交互界面会引导选择 provider、录入 API Key、配置模型，并**自动测试连通性**。兼容 **OpenAI 与 Anthropic** 协议，因此可指向自建网关。

### 评审

```bash
cd your-project

# 工作区模式 —— 评审所有已暂存/未暂存/未跟踪的改动
ocr review

# 分支区间 —— 评审 feature 分支自分叉点以来的改动（merge-base 模式）
ocr review --from main --to feature-branch

# 单次提交
ocr review --commit abc123

# 断点续评
ocr session list
ocr review --from main --to feature-branch --resume <session-id>

# 全文件扫描 —— 不走 diff，直接审整个文件（无需 git 历史）
ocr scan                          # 扫描整个仓库
ocr scan --path internal/agent    # 扫描指定目录或文件
ocr scan --resume <session-id>

# 结构化输出（推荐给宿主 Agent 消费）
ocr review --format json --output result.json

# 委派模式 —— 由你的编码 Agent 自行完成评审
# OCR 只负责选文件与规则解析，无需配置 LLM Key
ocr delegate preview
ocr delegate rule src/main.go src/handler.go
```

### 两种执行模式

| 模式 | 说明 |
|---|---|
| **默认（OCR 托管）** | 由 OCR 用自己配置的 LLM 执行评审 |
| **委派模式（Delegation）** | 由你的编码 Agent 用**它自己的 LLM** 执行评审，**无需 OCR 的 API Key**；OCR 仅负责文件选择与规则解析 |

## 宿主 Agent 集成

已适配并各自提供插件：

| 宿主 | 集成形态 |
|---|---|
| **Claude Code** | 插件 + review 斜杠命令 |
| **Codex** | 插件 + 可调用 review skills |
| **Cursor** | 插件 + 可移植 review skills |
| **Kimi Code** | 插件 + 斜杠命令与 skills |
| **OpenCode** | 原生 review 工具 + 斜杠命令 |
| **QCA Forward** | 委派模式 + 开箱可发布模板 |
| **Skill 兼容 Agent** | 可移植 agent skill |

## 配套能力

| 能力 | 说明 |
|---|---|
| **内置规则集** | 多语言规则，覆盖 NPE、线程安全、XSS、SQL 注入等 |
| **MCP Server** | 用外部工具扩展评审 Agent |
| **Session Viewer** | 浏览器中浏览与回放评审会话；把评论标记为**已修复/已忽略**，处理过程中可隐藏，便于逐条推进 |
| **会话导出** | 可导出为自包含的 HTML 文件 |
| **CI/CD 集成** | GitHub Actions、GitLab CI、**GitFlic CI**、**Gerrit** |
| **OpenTelemetry** | 可观测性集成 |
| **多语言文档** | README 提供简体中文、日本語、한국어、Русский |

## 使用场景

### ✅ 适用

- **有代码评审流程但覆盖不稳的团队** —— 大 PR 上通用 Agent 会漏审文件，该工具的「精确文件选择 + 智能打包」正是针对此
- **需要在 CI 里跑评审** —— `--format json` 结构化输出 + 官方 CI/CD 集成，便于做成流水线门禁
- **审计陌生代码库** —— `ocr scan` 全文件扫描不依赖 diff，适合接手遗留项目
- **已有自建 LLM 网关的组织** —— 兼容 OpenAI/Anthropic 协议，可直接指向内部网关复用已有模型与额度
- **评审需要留痕与可复盘的场景** —— Session Viewer + 会话导出 + OpenTelemetry，评审过程可回放、可审计
- **巨型改动集** —— 文件打包为子 Agent 分治，天然并发，规避上下文爆炸

### ❌ 不适用（避免硬套）

- **无 Git 历史的代码库** —— `ocr review` 依赖 diff；虽有 `ocr scan` 兜底，但核心工作流建立在 Git 上（且要求 Git ≥ 2.41）
- **期望零误报** —— 作者明确 Recall 低于通用 Agent，是**刻意取舍**；若你的场景「宁可多报也不能漏」，需评估此权衡
- **完全离线的环境** —— 需调用 LLM；若既要离线又要评审，只能自建模型服务并用自定义 provider 指向
- **非代码产物的评审** —— 文档、设计稿、业务规则变更不在其能力范围
- **把评审结论当最终裁决** —— 它产出的是**候选项**，仍需人工确认；尤其金融/生产环境

## 优势与特点

1. **生产验证过的出身** —— 阿里内部两年、数万开发者、数百万缺陷，非实验室作品。
2. **架构判断清晰** —— 「确定性工程 × Agent」的切分有明确依据（哪些必须不能错 vs 哪些需要动态判断），是**可复用的架构范式**，不止于这个工具本身。
3. **成本优势显著** —— 同模型下 token 仅约 1/9，因为工具的「聚焦」减少了无效上下文。
4. **基准透明** —— 自建 AACR-Bench 并公开数据集（50 仓库/200 PR/10 语言/80+ 工程师交叉校验/1,505 条真值），可自行复现核验。
5. **工程配套完整** —— CI/CD、MCP、OpenTelemetry、Session Viewer、多宿主插件、委派模式，是**可落地**的工具而非 demo。
6. **协议中立** —— 兼容 OpenAI 与 Anthropic，不被单一模型厂商绑定。
7. **治理信号好** —— Apache-2.0、OpenSSF Best Practices Gold、243 个 open issues 且当日仍在提交。

## 注意事项

- **自我评测**：Benchmark 章节的对比数据由项目方给出，虽数据集公开，选型前建议在自己代码库上实测。
- **Recall 取舍**：偏精确、少报噪音——若团队希望「多捕漏」，需自行评估是否需要补充手段。
- **243 个 open issues**：活跃项目常态，但接入前建议检索与你所用宿主（如 Cursor / Codex）相关的 issue。
- **英文化改造正在进行**：近期提交大量为「translate Chinese comments to English」，说明项目在向国际化收敛，中文注释相关的文档/代码可能处于变动期。
- **同名项目较多**：GitHub 上有多个 `open-code-review` 同名仓库（如 `spencermarx/open-code-review` ★367、`victortong-git/open-code-review` ★23、`opencodereview-org/opencodereview` ★2）。**本条收录的是 `alibaba/open-code-review`**，与同名者不是同一项目，引用时注意区分。

## 标签

`#代码开发工具` `#AI工程实践` `#AI研发流程改造` `#代码评审` `#Agent架构` `#CI-CD` `#确定性工程` `#OpenSSF-Gold`
