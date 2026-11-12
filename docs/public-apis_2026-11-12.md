# public-apis

> 收录时间：2026-11-12 | 原始链接：https://github.com/public-apis/public-apis | ⭐ 498,251 | 🍴 55,761

## 基础信息
| 项 | 内容 |
|----|------|
| 作者 | public-apis 社区（2016-03 建仓，10 年持续维护） |
| 开源协议 | MIT |
| 技术栈 | Python（校验脚本）+ Markdown（清单本体） |
| 所属领域 | 代码开发工具 |
| 核心用途 | 技术资源导航 |
| 最近更新 | 2026-11-11（仍在活跃推送） |
| 创建时间 | 2016-03-20 |
| 规模 | **52 个分类 / 2,415 条 API 记录**（README 约 356KB） |

## 项目简介

GitHub 上最大的**免费公共 API 清单**，也是全站 star 数排名前列的仓库之一（近 50 万 star、5.5 万 fork）。

它本身不是软件，是一个持续维护的 Markdown 目录：按分类列出"免费或带免费额度"的公共 API，每条给四列关键信息——**API 名称 / 简介 / 认证方式（apiKey、OAuth、No）/ HTTPS 与 CORS 支持**。

严格说它和「免费大模型额度」不是一类：`public-apis` 收录的是通用服务接口（天气、汇率、新闻、OCR、政府开放数据…），而 `free-llm-api-resources` 那类才有 LLM 免费额度。

## 核心功能

1. **52 个分类全覆盖**：Animals、Anime、Anti-Malware、Art & Design、Authentication、Blockchain、Books、Business、Calendar、Cloud Storage、Cryptocurrency、Development、Documents、Email、Finance、Government、Health、Machine Learning、News、Open Data、Patent、Photography、Programming、Security、Test Data、Text Analysis、Transportation、Weather 等。
2. **认证方式一目了然**：`No` 的可以直接调；`apiKey` / `OAuth` 需要先注册。
3. **附带 HTTPS / CORS 标记**：直接决定能不能在浏览器前端调用。
4. **自动化校验**：仓库带 Python 脚本对链接做存活与格式校验，`CONTRIBUTING.md` 规定了收录标准（必须免费或有免费层、必须可公开注册）。
5. **可编程消费**：清单是 Markdown 表格，正则或脚本很容易抽成 JSON 存进自己的服务目录。

## 使用方式

```bash
# 直接看
open https://github.com/public-apis/public-apis

# 抽成结构化数据（示例：抓 Finance 分类下的条目）
curl -s https://raw.githubusercontent.com/public-apis/public-apis/master/README.md \
  | awk '/^### Finance/,/^### Food/' | grep -E '^\| \[' | head
```

## 使用场景

- **找现成数据源**：做原型、内部工具、demo 时，先在这里找免费接口，省掉自己搭后端。
- **补齐测试数据**：`Test Data`、`Open Data`、`Government` 分类适合造样本数据。
- **评估第三方依赖**：列出的 `Auth` 与 `CORS` 两列能快速判断集成成本。
- **做自己的 API 导航站**：清单本身就是可解析的数据源（MIT，可自由使用）。

## 优势与注意事项

**优势**
- 10 年维护、5.5 万 fork，社区校验机制成熟，坏链会被 PR 修掉。
- 每条都带认证方式和 CORS 标记，比"只给个链接"的清单实用得多。
- MIT 协议，可自由抽取进自己的工具链。

**注意**
- **质量参差不齐**：约三分之一条目已失效或转收费，**用之前先 `curl` 探活**，别直接写进生产代码。
- **"免费"通常带限额**：多数是每月 N 次的免费层，不是无限用；商用前务必读该 API 自己的条款。
- **数据合规**：`Government`、`Health`、`Finance` 类接口的数据来源与可用范围各不相同，涉外或涉个人信息的数据要单独评估。
- **不是 LLM 额度清单**：找免费大模型 token 看 `free-llm-api-resources`、`awesome-free-llm-apis` 那两个。

## 标签

`#代码开发工具` `#技术资源导航` `#公共API` `#免费资源` `#MIT`
