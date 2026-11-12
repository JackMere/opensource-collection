# awesome-free-llm-apis

> 收录时间：2026-11-12 | 原始链接：https://github.com/mnfst/awesome-free-llm-apis | ⭐ 12,932 | 🍴 1,544

## 基础信息
| 项 | 内容 |
|----|------|
| 作者 | mnfst 社区 |
| 开源协议 | CC0-1.0（公共领域，可自由取用） |
| 技术栈 | JavaScript（数据整理脚本）+ Markdown |
| 所属领域 | AI大模型 |
| 核心用途 | 技术资源导航 |
| 最近更新 | 2026-11-11 |
| 创建时间 | 2026-03-21 |
| 收录口径 | **只收「永久免费层」**，不含试用额度、不含限时活动 |

## 项目简介

一份只收 **永久免费层（permanent free tier）** 的 LLM API 清单——这是它与其它同类最大的区别：不收录"注册送 5 美元"这类一次性额度，因此列表稳定、可用性高。

结构上分两大类：

- **Provider APIs**：模型厂商自己提供的免费层（Google Gemini、Cohere、Aion Labs、智谱 Z AI 等）
- **Inference providers**：推理服务商提供的免费额度（Groq、NVIDIA NIM、Cloudflare Workers AI、OpenRouter、SiliconFlow、ModelScope、OVHcloud 等）

每个条目给出**模型 ID、上下文长度、最大输出、模态（含 reasoning 标记）、限流（RPM/TPD）**，并附该国旗帜与直达 API Key 申请页的链接。

## 核心功能

1. **表格化规格**：117 行表格，逐模型列上下文 / 最大输出 / 模态 / 限流，可直接当选型依据。
2. **直达申请入口**：每个提供商标题就是它自己的 API Key 页面链接，免去翻文档。
3. **标注 OpenAI 兼容性**：说明「多数提供商提供 OpenAI 兼容端点，但原生 base URL 与兼容 base URL 可能不同」——这是实际接网关时最容易踩的坑。
4. **地区标识**：🇺🇸 🇨🇳 🇮🇱 🇨🇦 🇫🇷 🇬🇧 等，便于判断国内可达性。
5. **CC0 授权**：公共领域，数据可任意抽取使用，无署名义务。

## 使用方式

```bash
# 直接看
open https://github.com/mnfst/awesome-free-llm-apis

# 抽出所有含 reasoning 的模型（示例）
curl -s https://raw.githubusercontent.com/mnfst/awesome-free-llm-apis/main/README.md \
  | grep -E '^\| `' | grep -i reasoning | head
```

## 使用场景

- **给 New API / 网关加免费渠道**：按表格里的供应商逐个申请，拿到 key 直接配渠道。
- **选型时比限流**：同样的免费层，Groq 的 RPM 和 Gemini 的 TPD 差很多，表格能直接对比。
- **判断国内可达性**：看旗帜 + 是否提供 OpenAI 兼容端点。
- **做自建免费额度池**：CC0 授权，可以把清单抽成 JSON 灌进自己的管理系统。

## 优势与注意事项

**优势**
- **只收永久免费层**，比"注册送额度"型清单可靠得多，列表噪音低。
- 规格列齐全（上下文、最大输出、模态、限流），选型效率高。
- CC0 授权，商用/二次分发无约束。

**注意**
- **限流随时会变**：RPM/TPD 是编写时点的数据，厂商调整不通知。
- **免费层通常"数据用于训练"**：别把内部文档、客户资料喂给免费层。
- **国内可达性要实测**：标了 🇨🇳 的才是国内平台；海外平台的 API 端点在国内常常连不上。
- 免费层普遍**不支持商用或 SLA 无保证**，做生产依赖前先看各自条款。

## 标签

`#AI大模型` `#技术资源导航` `#免费LLM` `#API额度` `#CC0`
