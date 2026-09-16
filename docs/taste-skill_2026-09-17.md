# taste-skill

> 收录时间：2026-09-17 | 原始链接：https://github.com/Leonxlnx/taste-skill | ⭐ 87,680 | 🍴 5,965

## 基础信息
| 项 | 内容 |
|----|------|
| 作者 | Leonxlnx |
| 开源协议 | MIT |
| 技术栈 | JavaScript、Shell（约束对象为 React / Next.js / Tailwind v4） |
| 所属领域 | 前端设计、代码开发工具、AI工程实践 |
| 核心用途 | 前端界面生成提质 |
| 最近更新 | 2026-09-16 |
| 创建时间 | 2026-02-19（约 7 个月达成 8.7 万星） |
| 官网 | https://tasteskill.dev |
| 技能数量 | 13 个（9 个出代码 + 3 个只出图 + 1 个防截断） |
| 主技能规模 | `skills/taste-skill/SKILL.md` 1,206 行 / 87 KB |

## 项目简介

taste-skill 是一套**可移植 Agent Skills 集合**，目标只有一个：**给 AI 加「审美」，阻止它生成千篇一律的模板化前端界面**（作者称之为 anti-slop，反「AI 味」）。

它约束的对象是**AI 生成前端时的行为**，不是给人看的组件库。核心机制是「先读懂需求，再动手」——技能要求 AI 在写第一行代码前，先输出一行「设计读解」（Design Read），推断页面类型、氛围词、参考物、受众与隐性约束，然后据此配置三个旋钮再生成。

作者对它的定位非常克制且诚实：**只做营销型页面（落地页 / 作品集 / 改版），并明确列出不适用的场景清单。**

## 核心功能

1. **三旋钮配置机制（§1）**：`DESIGN_VARIANCE`（视觉变化度 1-10）/ `MOTION_INTENSITY`（动效强度 1-10）/ `VISUAL_DENSITY`（信息密度 1-10），基线 8 / 6 / 4，按「设计读解」自动推出，无需用户手工改文件（支持对话内覆盖）。

2. **Brief 读解前置（§0）**：动手前先读 6 类信号（页面类型、用户说的氛围词、参考链接/截图、受众、既有品牌资产、隐性约束），输出一行设计读解。**模糊时只问一个问题，不猜**；能从上下文推断就不问。

3. **机械可验的前置检查（§14）**：这是它最硬核的设计——检查项是**可以数数的**，而非主观判断。例如：
   - 眉标（eyebrow）数量 ≤ 章节数 ÷ 3
   - CTA 按钮文案在桌面端不得换行
   - 同一意图的 CTA 不允许出现两个（"Get in touch" + "Let's talk" = 失败）
   - Z 字交替布局最多连续 2 段，第 3 段即失败
   - 全页 em-dash（`—`）数量必须为 **0**

4. **156 处「AI 味」禁令（§9）**：全部来自真实 LLM 输出测试的总结，例如禁止 hero 区版本号标签、禁止 `001 · Capabilities` 式章节编号、禁止 div 拼的假产品截图、禁止装饰性彩色状态点、禁止「Quietly in use at」式虚浮社会证明。

5. **需求 → 官方设计系统映射（§2）**：明确指向官方包而非手抄 CSS——Fluent（微软/企业级）、Material 3（Google 味）、Carbon（IBM 风 B2B）、Polaris（Shopify 后台）、Atlassik（Jira 风）、Primer（GitHub 风）、GOV.UK（英国公共服务，法律/监管要求）、USWDS（美国政务）、Bootstrap、Radix、shadcn/ui、Tailwind。配套硬规矩：不许手抄官方系统 CSS；一个项目只用一个系统。

6. **改版风险协议（§11）**：区分 Greenfield / Preserve（保品牌现代化）/ Overhaul（换视觉）三种模式，要求**先审计再动手**，并明确列出不许静默变更的项——URL 结构、锚点 ID、导航文案、表单字段名、品牌色、法律文案、埋点事件名。理由直白：**「SEO 迁移是重做网站的头号风险」**。

7. **性能与无障碍护栏（§6）**：强制 `prefers-reduced-motion`、消费级页面强制暗色模式、Core Web Vitals 目标、硬件加速、z-index 克制。

8. **依赖校验（§3.F）**：导入任何三方库前必须查 `package.json`，缺了就输出安装命令，**不许假设库存在**。

9. **大模型输出截断研究（`research/laziness/`）**：仓库内含原创实证研究，分析模型为何产生不完整输出（占位代码、截断回复、跳过章节），覆盖根因（RLHF 与算力经济学、训练数据偏差、认知捷径、输出上限）、补救（参数调优、提示工程、架构模式、参考提示词）与 2025 年学术研究的实证数据。

## 技能清单（13 个）

**出代码类（9）**

| 技能 | 安装名 | 场景 |
|------|--------|------|
| taste-skill | `design-taste-frontend` | **默认首选**，落地页/作品集/改版（v2 实验版） |
| taste-skill-v1 | `design-taste-frontend-v1` | v1 原版，供依赖其精确行为的项目锁定 |
| gpt-tasteskill | `gpt-taste` | 面向 GPT/Codex 的更严格变体，Awwwards 级 + GSAP 动效 |
| image-to-code-skill | `image-to-code` | 先出设计图 → 深读图 → 按图实现 |
| redesign-skill | `redesign-existing-projects` | 改进已有项目：先审计再按优先级修 |
| soft-skill | `high-end-visual-design` | 高端代理商质感：贵气字体、留白、深度、柔和动效 |
| minimalist-skill | `minimalist-skill` | 极简编辑风（Notion/Linear 系），严格单色 |
| brutalist-skill | `brutalist-skill` | 粗野主义：瑞士排版 + 军规终端审美（Beta） |
| output-skill | `full-output-enforcement` | 跨场景可用：专治 AI 偷懒截断与占位注释 |

**只出图类（3）**：`imagegen-frontend-web`（网站设计稿）、`imagegen-frontend-mobile`（移动端界面与流程）、`brandkit`（品牌套件板：logo 方向、配色、字体、识别应用）。配合 ChatGPT Images 或 Codex 图像模式出参考图，再交给编码 agent 实现。

**另有**：`stitch-skill` —— 为 Google Stitch 生成 DESIGN.md 语义设计规范。

## 安装与使用

```bash
# 安装整套（npx skills CLI 会扫描 skills/ 目录）
npx skills add https://github.com/Leonxlnx/taste-skill

# 只装单个技能（用 frontmatter 里的 install name，不是文件夹名）
npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend"

# Claude Code 插件方式
claude plugin marketplace add leonxlnx/taste-skill
claude plugin install taste-skill@Leonxlnx/taste-skill

# 也可直接把 SKILL.md 拷进项目，或粘贴到 ChatGPT / Codex 对话里
```

## 使用场景（官方预设）

`§1.B Use-Case Presets` 是它自认的场景清单，三旋钮随场景自动配置：

| 场景 | 变化度 | 动效 | 密度 |
|------|-------|------|------|
| 落地页 · SaaS 主流 | 7 | 6 | 4 |
| 落地页 · 代理商/创意 | 9 | 8 | 3 |
| 落地页 · 高端消费品 | 7 | 6 | 3 |
| 作品集 · 设计师/工作室 | 8 | 7 | 3 |
| 作品集 · 开发者 | 6 | 5 | 4 |
| 编辑页 / 博客 | 6 | 4 | 3 |
| 公共服务 / 政府站点 | 3 | 2 | 5 |
| 改版 · 保留品牌 | 沿用 | +1 | 沿用 |
| 改版 · 换视觉 | +2 | +2 | 沿用 |

## 明确不适用的场景（§13 OUT OF SCOPE）

| 排除项 | 官方建议替代 |
|--------|-------------|
| 仪表盘 / 密集产品 UI / 管理后台 | Fluent UI、Carbon、Atlassian、Polaris |
| 数据表格 | TanStack Table、AG Grid |
| 多步表单 / 向导 | 表单专门模式（技能自述「帮不上」） |
| 代码编辑器 | Monaco / CodeMirror 官方皮肤 |
| 原生移动端 | Apple HIG / Material 直接上 |
| 实时协作 UI（在线状态、光标） | 属另一类问题 |

技能的处理方式很诚实：**若需求属上述范围，就明说「不在我范围内」并指向对的工具**，只在营销页面部分复用。

## 优势与特点

### 优势
- **规则可机械验证**，不靠模型主观判断——前置检查是数数（眉标数量、CTA 数量、布局重复次数），这在同类技能里少见
- **156 条禁令源自真实 LLM 输出测试**，不是拍脑袋写的审美偏好
- **对官方设计系统诚实**：指向官方包而非手抄，且明确标注「Apple Liquid Glass 只是近似，无官方 CSS」
- **改版协议保护 SEO 与分析埋点**，明确列出不许静默变更的契约项
- **附带原创实证研究**（大模型输出截断根因与补救），超出一般技能集的信息密度
- **MIT 协议**，无商用限制；作者另挂防诈声明澄清与任何代币无关
- **跨 agent 通用**：兼容 Claude Code、Codex、Cursor、Gemini CLI、Windsurf、v0、Lovable 等

### 注意事项
- **主技能 v2 仍是 experimental（实验版）**，作者自述规则措辞可能还会变，稳定版为 v2.0.0 尚未发布；依赖精确行为的项目应锁定 `taste-skill-v1`
- **技术栈强绑定**：假设 React / Next.js（RSC）+ Tailwind v4 + Motion，非此栈需自行翻译规则
- **场景窄**：只做营销型页面，明确排除仪表盘、数据表格、多步表单、代码编辑器、原生移动端
- **主 SKILL.md 达 87 KB**，全量载入对上下文占用可观，建议按需只装单个技能
- **同名项目多**：仓库内另有 senlindesign/taste-skill（逆向网站设计 token）、codeswithroh/tastemaker（用真实参考图 + 开发者审美画像）等不同作者的同名项目，注意区分

## 标签
`#前端设计` `#反AI味` `#Agent-Skills` `#UI生成` `#Tailwind` `#React` `#设计系统` `#落地页` `#提示工程` `#MIT` `#anti-slop` `#GSAP`
