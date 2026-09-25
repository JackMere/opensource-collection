# codegraph
> 收录时间：2026-09-25 | 原始链接：https://github.com/colbymchenry/codegraph | ⭐ 72,067 | 🍴 4,623

## 基础信息
| 项 | 内容 |
|----|------|
| 作者 | colbymchenry |
| 开源协议 | MIT |
| 技术栈 | C、Rust、TypeScript |
| 所属领域 | 代码开发工具、AI工程实践、知识库管理 |
| 核心用途 | Token压缩与上下文优化、代码理解与智能检索 |
| 最近更新 | 2026-09-23 |
| 创建时间 | 2026-01-18 |
| 文档站点 | https://colbymchenry.github.io/codegraph/ |
| npm | `@colbymchenry/codegraph` |

## 项目简介
CodeGraph 是预索引式代码知识图谱。Rust 内核 + tree-sitter 抽取每个符号、调用边、依赖边，解析与抽取全程无 LLM 参与，存入本地 SQLite。Agent 通过一次 MCP 调用即可拿到精确源码、符号间调用路径（含 grep 跟不动的动态派发跳转）与改动影响半径，避免逐文件爬取。明确支持 C++（`.cpp/.hpp/.cc` Full support，google/leveldb 实测跨文件覆盖 94.8%），另支持 C、CUDA、Go、Rust、Java、Python、TypeScript 等 30+ 语言。

## 核心功能

1. **Surgical Context（外科式上下文）**：一次调用返回入口点、相关符号与代码片段，Agent 无需逐文件探索
2. **影响半径分析**：追踪任意符号的 callers、callees 与完整影响范围，改动前先看谁会炸
3. **自动同步**：FSEvents / inotify / ReadDirectoryChangesW 原生文件监听，去抖后自动重索引；连接时用 `(size, mtime)` + 内容哈希补齐离线期间的变更
4. **30+ 语言统一图谱**：跨文件解析进同一张图，无逐语言额外配置
5. **全本地**：无数据外传、无 API Key、无外部服务，仅 SQLite
6. **MCP 服务**：一次 `codegraph serve --mcp` 接入，工具返回含 ⚠️ 陈旧文件横幅防静默错误答案
7. **框架感知路由**：识别 17 个 Web 框架路由文件，把 URL 模式连到 handler

## 兼容矩阵 / 支持的 Agent

- **Hermes Agent**：✅ README 明确列出
- **Claude Code / Codex / OpenCode / Cursor**：✅
- **Gemini CLI / Antigravity / Kiro / GitHub Copilot**：✅
- **语言**：TypeScript、JavaScript、ArkTS、Python、Go、Rust、Java、C#、VB.NET、PHP、Ruby、C、**C++**、**CUDA**、Objective-C、Metal、Swift、Kotlin、Scala、Dart、Vue、Svelte、Astro、Lua、Luau、R、Nix、Erlang、CFML、COBOL、Solidity、Terraform/OpenTofu、Liquid、Pascal/Delphi

## 安装与使用
```bash
# Linux / macOS（无需 Node.js）
curl -fsSL https://raw.githubusercontent.com/colbymchenry/codegraph/main/install.sh | sh

# 或 npm
npm i -g @colbymchenry/codegraph

# 项目内初始化 + 索引
codegraph init
codegraph serve --mcp
```

## 基准数据（作者自测，2026-08-05 重测，Claude Opus 4.8）

| 指标 | 结果 |
|------|------|
| 工具调用 | -88% |
| 耗时 | -53% |
| Token | -62% |
| 成本 | -44% |
| 文件读取 | 7 个测试库全部降为 0 |

诚实缺陷：
- 7 个基准库**不含 C++**（VS Code/Excalidraw/Django/Tokio/OkHttp/Gin/Alamofire），C++ 94.8% 是抽取覆盖率非任务准确率
- **长会话残留上下文更高**：结束时比纯 grep-Read 多约 80% token 驻留（VS Code 上 67k vs 18k）——1M 窗口可承受，但机制需知晓
- 动态派发、宏重度、模板元编程的 C++ 仍可能漏边
- 572 个 open issue，8 个月新项目
- ⚠️ 作者有遥测（见 `TELEMETRY.md`），敏感环境需评估

## 适用场景

- 大型存量代码库的日常维护（10 万行 + C++/Java/Go 混合栈）
- 改动前的影响面评估、"改这个函数会波及哪些地方"
- Agent 冷启动时快速建立代码库全局认知
- 降低长会话的 token 成本与工具调用轮次

## 不适用场景

- 需要"代码里读不出来的业务语义/历史包袱"——那是 AOCI-CODE 的 S 字段，不是静态符号图能给的
- 强模板元编程/宏重度 C++ 且要求边级完备（静态分析天花板）
- 小于几千行的项目（收益不明显，索引开销反而是负担）
- 需要 Git 版本化、可 diff/回滚的索引资产（CodeGraph 索引在 SQLite，不进 Git）

## 标签
`#代码开发工具` `#AI工程实践` `#知识库管理` `#Token压缩与上下文优化` `#代码理解与智能检索` `#MCP` `#代码知识图谱` `#tree-sitter` `#C++` `#本地优先`
