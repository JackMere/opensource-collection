# The AI-Native SDLC Playbook

> 收录时间：2026-09-15 | 原文链接：https://claude.com/blog/the-ai-native-sdlc-playbook | 发布方：Anthropic | 发布时间：2026-08-21

> ℹ️ **本页为结构化精读导读（原文约 48 分钟阅读 / 79,699 字符），非全文转载**——原文版权归 Anthropic 所有，请通过上方链接阅读全文。导读覆盖核心论点、六阶段框架、16 个 play 清单、关键机制与度量指标。

## 基础信息
| 项 | 内容 |
|----|------|
| 标题 | The AI-Native SDLC playbook |
| 副标题 | How to transform your software development lifecycle with AI—stage by stage |
| 发布方 | Anthropic（Claude 官方博客） |
| 作者 | Louis Claxton（致谢 Jim Blackhurst、Will Steuk、Jamal Arif） |
| 发布时间 | 2026-08-21 |
| 类别 | Enterprise AI、Claude Code |
| 篇幅 | 约 48 分钟阅读 / 79,699 字符 |
| 原文链接 | https://claude.com/blog/the-ai-native-sdlc-playbook |
| 官方课程版 | https://academy.claude.com/courses/ai-native-sdlc-playbook（14 节课） |
| EPUB 备份 | https://github.com/yibie/ai-native-sdlc-playbook |
| 所属领域 | AI工程实践 |
| 核心用途 | AI研发流程改造 |

## 核心论点：代码不再是瓶颈，SDLC 才是

开篇论断是全文的支点。论证链条：

> 让「构建」阶段快 10 倍，吞吐量**不会**涨 10 倍——工作会堆在**评审、测试、发布**前面，而这三者仍按人力速度运行（每周或每月开一次会）。

由此推出三个后果：

1. **瓶颈向两侧迁移** —— 主要落在 Plan / Review+Test / Deploy
2. **控制手段与现实脱节** —— 「逐行人工审查」在人工写代码时代合理，但 agent 写了大部分 diff 后就跑不动了
3. **治理成本上升** —— 例外仍要走进每周开会的委员会

原文用安全评审做例子：安全团队按人类产出速度配置人员，agent 一旦放大代码产出，则**要么评审队列堆积、要么代码未充分评审就上线**。受监管组织两种结果都不能接受，所以安全与策略检查必须跟上 agent 的速度。

**结论**：AI 原生 SDLC 不是「更快的 SDLC」，而是**把评审前移**——从审 diff 转向**审意图**。

## 结构性变化：从线性流程到闭环回路

传统 SDLC 是线性流程，各阶段由不同角色拥有，靠文档/工单/签核流转。AI 原生版要求：**每个阶段结束时提交一个产物（artifact）到版本控制，下一阶段读取它开始工作。**

```
intent.md → spec.md → plan.md → diff+tests → PR+审查结论 → 事故记录
   ↑                                                          │
   └──────────  生产控制带被突破，写回新的 intent.md  ←─────────┘
```

**提交链本身就是审计链**：谁提了什么、agent 产出了什么、谁批准的，全部记录在 git 中。

## 六阶段对照表（原文核心表）

| 阶段 | 传统 SDLC | AI 原生 SDLC |
|------|-----------|--------------|
| **Plan** | 委员会收集需求，工作坊 + 签核，人工撰写 | Claude 直接从原始材料综合痛点，写入 `intent.md`（人可读 + 机器可执行） |
| **Design** | 分析师写规格，设计师再解析一遍 | 需求与设计**压缩进一次会话**，由编码为 skills 的标准约束，git 版本管理 |
| **Build** | 测试与代码手写，文档事后补 | AI 生成代码与测试，机构知识维护为版本化、机器可读的 `CLAUDE.md` 与 skills |
| **Test** | 阶段边界的 QA 关卡 | **持续评测（evals）** 编织进实现过程 |
| **Deploy** | 人审每一行，治理发生在评审周期里（常不一致） | **多层 agent 审查**，人工审查保留给受监管/关键代码；治理由 **hooks 作为审批闸门**在 AI 行动时强制执行 |
| **Maintain** | 人盯着生产找 bug | Agent 监控线上部署，**控制带被突破即诊断并写回新的 `intent.md`** |

## 16 个 Play（按阶段）

每个 play 统一五段式：**改变什么 → 如何起步 → 具体实施步骤 → 治理考量 → 如何度量是否有效**。且 play 是**模块化、非线性**的，各自标注「前置依赖」，组织可按需选择优先改造哪个阶段。

| 阶段 | Plays |
|------|-------|
| **1 Plan** | Capture as `intent.md` |
| **2 Design** | Requirements and design（需求 + 设计合并为一次会话） |
| **3 Build** | Claude Code plan mode 作为默认起点；`CLAUDE.md`；Skills 作为机构知识；Hooks 作为构建期护栏；并行会话与 subagents（另含 auto 模式、遗留系统「唯一事实源」选择） |
| **4 Test** | 给 Claude 一个反馈闭环（自验：跑测试/构建/截图）；CI 中的持续 evals |
| **5 Deploy** | PR 审查回路中的 AI；Hooks 作为审批闸门；受监管企业的 managed settings；CI/CD 集成与部署 |
| **6 Maintain** | 闭环（控制带突破 → 触发 → 写回 `intent.md`）；周期性代码库扫描；Claude 值班（Claude Tag） |

## 关键机制精读

### 1. `intent.md` —— 把意图变成产物
发起人用自己的话与 Claude 头脑风暴，产出 proto-spec：**要什么、为什么、受什么约束**。由产品负责人审核修正后提交到共享的版本化「意图之家」（单产品就是 repo 里的 `intent/` 目录）。**重复性流程编码为 skills。**

对比传统：想法要经过 backlog、用户故事、故事点、细化会议才能被行动；每次交接都转移所有权，到达工程侧时已偏离原始意图好几步。

### 2. `CLAUDE.md` —— 机构知识成为 agent 读的文件
给 Claude 提供「新加入者所需的上下文」：约定、命令、架构，以及**团队最常犯的错误**。过去存在人脑里的知识变成版本化、机器可读的文件。**评审发现的错误第二次出现时，修正直接写进 `CLAUDE.md`**——由于评审会读它，从下一个 PR 起该错误就被拦住。

### 3. Skills vs Hooks —— 全文最重要的区分 ⭐

| | Skills | Hooks |
|---|---|---|
| 性质 | **建议性控制**（advisory） | **确定性控制**（deterministic） |
| 作用 | 让 agent **倾向于**遵守策略 | **强制**阻止或询问 |
| 判定 | 无法保证会话一定合规 | 每次都执行，不依赖模型判断 |
| 使用 | 编码必须一致适用的机构知识 | 有策略必须**无例外**成立时，用 hook 兜底 |

**Build 阶段** hook 可做：阻止修改受保护路径（生成代码/冻结包）、文件编辑后跑格式化与 linter、把凭据挡在 diff 之外。注意 build 期 hook 应**快且只针对变更文件**；较重的检查（全量测试）应放在 commit 或 PR 阶段。

**Deploy 阶段** hook 转为**审批闸门**：team hook 放 `.claude/settings.json`（进 git），不可协商的 hook 放 managed settings（平台/IT 管理，工程师无法关闭）。**拦截必须自我解释**——hook 阻止动作时，原因与审批路径要出现在 Claude 输出中。

原文示例（生产闸门）：
```json
{
  "hooks": {
    "PreToolUse": [
      { "matcher": "Bash",
        "hooks": [{ "type": "command",
                    "command": "${CLAUDE_PROJECT_DIR}/.claude/hooks/production-gate.sh" }] }
    ]
  }
}
```
```bash
#!/bin/bash
# Production deploys require a named release authorization
cmd=$(jq -r '.tool_input.command' < /dev/stdin)
if [[ "$cmd" == *"deploy"* && "$cmd" == *"production"* ]]; then
  if [ -z "$RELEASE_APPROVAL" ]; then
    echo "Production deploys need a release authorization." >&2
    exit 2   # exit 2 blocks the action; the message goes to Claude
  fi
fi
exit 0
```

### 4. 受监管企业的 Managed settings（原文最实用的配置）
由平台团队通过 MDM 或管理控制台部署，**工程师无法编辑或覆盖**：
```json
{
  "permissions": {
    "deny": ["Read(.env*)", "Read(./secrets/**)", "WebFetch", "Bash(curl *)", "Bash(wget *)"],
    "allow": ["Bash(git *)", "Bash(make build)", "Bash(make test)", "Bash(make lint)"],
    "disableBypassPermissionsMode": "disable"
  },
  "allowManagedPermissionRulesOnly": true,
  "sandbox": {
    "enabled": true,
    "failIfUnavailable": true,
    "allowUnsandboxedCommands": false,
    "network": { "allowedDomains": ["git.internal.example.com", "registry.npmjs.org"] },
    "credentials": {
      "files": [{ "path": "~/.ssh", "mode": "deny" },
                { "path": "~/.aws/credentials", "mode": "deny" }],
      "envVars": [{ "name": "GITHUB_TOKEN", "mode": "deny" }]
    }
  },
  "allowManagedHooksOnly": true,
  "disableSideloadFlags": true,
  "allowManagedMcpServersOnly": true,
  "strictKnownMarketplaces": [{ "source": "github", "repo": "example-corp/approved-plugins" }],
  "requiredMinimumVersion": "2.1.193"
}
```
**每一项的控制意义**（原文逐条解释）：
- `permissions.deny` —— 把密钥挡在 agent 上下文外，阻断任意网络出口；`allow` 预先批准安全内循环，避免 deny 列表变成「提示疲劳」
- `disableBypassPermissionsMode` + `allowManagedPermissionRulesOnly` —— **任何工程师、项目文件或命令行参数都无法放宽规则**
- `sandbox` —— 补上权限管不到的缺口：工具级 `WebFetch` 拦截挡不住 shell 命令联网，**OS 级域名白名单**才能彻底阻断
- `failIfUnavailable` + `allowUnsandboxedCommands` —— 让沙箱成为闸门：沙箱起不来 Claude Code 就拒绝启动，沙箱内失败的命令不能在沙箱外重试
- `credentials` —— `permissions.deny` 只管 Claude 的文件工具，沙箱 shell 默认仍可读 `~/.ssh`；此块拒绝这些读取并从每个沙箱命令的环境中剥离指定密钥
- `allowManagedHooksOnly` —— **只有本 play 的审批闸门会运行，本地任何东西无法新增或替换**
- `strictKnownMarketplaces` —— 工程师机器上的每个 skill 与 MCP server 都必须来自已批准渠道

### 5. 持续 evals —— 回归测试 agent 的配置本身
- 从近期真实工作中收集 **20–50 个任务**及其被接受的结果
- 每个任务写成 eval：prompt + 判定可接受的检查（测试通过、lint 干净、行为不变、策略遵守）
- **在 `CLAUDE.md`、skills 或 hooks 发生任何变更时运行**——因为这套配置在引导 agent，值得像代码一样做回归测试
- 用结果**门控配置变更**：使通过率下降的 skill 变更必须经评审才能合并
- **每次生产事故新增一个 eval**，由事故归属团队编写，永久留在套件中

### 6. 闭环（Stage 6）—— 检测保持确定性
- 用**版本化脚本**（非模型）监控具有稳定基线的指标：CI 失败率、部署后 5xx 率、PR 周期时间
- 规则通常为滚动窗口上的均值与标准差（Western Electric 规则等）；**分档**：1σ 仅记录，2σ 以只读方式调用 agent 诊断，3σ 允许 agent 行动——但**只能开 PR 进评审闸门，或触发预先批准的 runbook**
- agent 把发现写成 **Stage 1 格式的 `intent.md`**，回路自我供给
- 关键设计：**最高自主档仍终止于 Stage 3 建的闸门、依赖 Stage 4 演练过的回滚、产出 Stage 1 定义的产物**

### 7. 「唯一事实源」的三种配置（遗留系统共存）
每个产物都要指定**一个**系统为事实源，其余持有副本或链接。三种可选配置：
- **以 repo 为事实源** —— Markdown 产物是权威记录，遗留系统在提交中引用文件；适合工程主导的组织（单一时间戳权威）
- **以遗留系统为事实源** —— Jira / ServiceNow / 需求工具持有权威记录，Markdown 是工作副本；Claude 在会话开始时读取，并在**同一会话内**通过 MCP 连接器把结果写回
- **链接为最低标准** —— 所有产物记录 record ID，所有遗留记录包含 Markdown 文件的 commit SHA。**过渡期推荐从此起步**（此时存在两个事实源）

## 度量指标（每个 play 均给出领先 + 滞后指标）

| Play | 领先指标 | 滞后指标 |
|------|----------|----------|
| Plan | 首次对话 → 提交 `intent.md` 的时间（从 git 历史读），预期从数周缩短到**数小时** | `intent.md` 存活率（被接受进入 Stage 2 的比例）；首个 `spec.md` 提交后 `intent.md` 仍被改动的次数 |
| Design | `intent.md` 提交 → `spec.md` 提交的间隔（两个 git 时间戳） | build 开始后的需求返工：统计晚于首个 `plan.md` 提交的 `spec.md` 提交数 |
| Build | 首次实现即合并的变更占比；计划批准 → PR 合并的时间 | 每次变更的返工轮次；合并的 diff 与已提交 `plan.md` 的吻合度 |
| Deploy | 无需呼人干预即完成分诊的流水线故障占比 | DORA 指标（CI 与部署工具已在产出） |
| Maintain | 已连接仓库按计划扫描的比例；发现 → 补丁进 PR 闸门的时间 | 计划扫描发现的漏洞 vs 生产/外部报告的漏洞；多次扫描后每轮发现数的下降趋势 |

## 治理的三条机械保障

1. **分支保护** —— agent 写的东西只能是 PR，**无路直达 main**；且**写代码的 agent 不能批准自己的代码**（职责分离靠机制保证）
2. **生产部署 hook** —— 无指定 release manager 授权则阻断发布
3. **Agent 自己的身份** —— 非交互运行以 agent 身份执行，流水线日志可区分「agent 做的」与「触发者做的」

## 使用场景

1. **评估自组织 AI 研发流程改造**：用六阶段框架盘点现状，定位瓶颈在 Plan / Review / Deploy 的哪一环
2. **设计 agent 治理策略**：直接取用 Skills（建议）vs Hooks（强制）的区分，与 managed settings 配置模板
3. **建立 agent 配置回归测试**：参照持续 evals 的 20–50 任务套件设计
4. **编写机构知识文件**：参照 `CLAUDE.md` / `SKILL.md` 的写法与维护机制
5. **对接遗留系统**：参考「唯一事实源」三种配置，处理 Jira/ServiceNow 与 Markdown 产物共存
6. **设计自动闭环运维**：参照控制带分档 + 检测保持确定性（脚本而非模型）的设计

## 优势与特点

### 优势
- **Anthropic 官方出品**，源自其 Applied AI 团队服务客户的真实最佳实践，非理论推演
- **可操作性极强**：每个 play 都给出前置条件、基础设施要求、编号步骤、治理考量和度量指标
- **直面受监管企业约束**：专门讨论审计、职责分离、合规、遗留系统共存，而非只谈效率
- **配置即文档**：`CLAUDE.md`、`SKILL.md`、`REVIEW.md`、`settings.json`、`production-gate.sh`、`agent-evals.yml` 等完整片段可直接参考
- **度量不空谈**：指标多数可从 git / PR 元数据 / CI 直接读取，无需新建仪表盘
- **配套官方课程**（14 节课），可系统学习

### 注意事项
- **强绑定 Claude Code / Anthropic 生态**：play 本身声称工具无关，但示例与配置全部基于 Claude Code / Claude Tag
- **面向大型与受监管企业**：原文明确说明「假设你能改变组织的规划、评审、测试、部署方式」，小团队直接套用可能过重
- **未涉及成本度量**：全文度量 cycle time 与 DORA 指标，**从未度量成本**（第三方评论指出这是明显缺口，token 消耗会随 eval 套件、多轮审查、验证 subagent 显著放大）
- **需要真实落地顺序**：play 间有依赖关系，乱序推进（如未建闸门就开并行 auto 模式）反而制造事故
- **中文资料较少**：目前社区二次解读多为英文

## 相关资源
- 原文：https://claude.com/blog/the-ai-native-sdlc-playbook
- 官方课程（14 课）：https://academy.claude.com/courses/ai-native-sdlc-playbook
- EPUB 备份：https://github.com/yibie/ai-native-sdlc-playbook
- 相关文章：Claude Code 官方文档（plan mode、skills、hooks、sandboxing、managed settings、MCP）

> ⚠️ **注意区分同名内容**：另有 2024 年 arXiv 论文《The AI-Native Software Development Lifecycle: A Theoretical and Practical New Methodology》（Cory Hymel，提出 **V-Bounce 模型**，arXiv:2408.03416，由 Crowdbotics 赞助），与本篇 Anthropic 工程实践手册是**不同的两份材料**。

## 标签
`#AI原生SDLC` `#agentic-SDLC` `#研发流程` `#ClaudeCode` `#Anthropic` `#intent.md` `#CLAUDE.md` `#skills` `#hooks` `#evals` `#治理与合规` `#企业级AI`
