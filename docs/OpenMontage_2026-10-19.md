# OpenMontage

> 收录时间：2026-10-19 | 原始链接：https://github.com/calesthio/OpenMontage | ⭐ 69,781 | 🍴 9,036

## 基础信息

| 项 | 内容 |
|----|------|
| 作者 | calesthio |
| 开源协议 | AGPL-3.0 |
| 技术栈 | Python（主）、HTML、JavaScript（Remotion 合成层） |
| 所属领域 | AI大模型、代码开发工具 |
| 核心用途 | 视频生成与剪辑 |
| 最近更新 | 2026-10-16 |
| 创建时间 | 2026-03-29 |
| 文档站点 | https://www.openmontage.video |

## 项目简介

OpenMontage 是一套开源的 **Agentic 视频制片系统**：把 Claude Code、Cursor、Copilot、Windsurf、Codex 这类编码助手，变成一个从选题研究到成片交付的完整制片厂。它由 12 条生产线、100 多个工具、700 多份「导演技能」（director skill，即教 agent 如何执行某个环节的 markdown 指令文件）组成，最终由 Remotion 合成层渲染出片。

与「给一段 prompt 生成一段视频」的模型类工具不同，它的定位是**流程编排**：把制片拆成可检查的阶段，每阶段有专门的技能文档、状态检查点和人工确认节点，最后跑多点多自检再交付。

## 核心功能

1. **11 类生产线**（每条是从想法到成片的完整工作流）：
   - Animated Explainer —— 研究 + 旁白 + 配乐动画解说
   - Animation —— 动态图形、动力排版，适合社媒与产品演示
   - Avatar Spokesperson —— 数字人口播
   - Cinematic —— 预告片、品牌片、情绪剪辑
   - Clip Factory —— 从长素材批量切出排好序的短视频
   - Documentary Montage —— 只取免费实拍素材（Pexels / Archive.org / NASA / Wikimedia / Unsplash）做检索式混剪，**不需要付费生成 API**
   - Hybrid —— 实拍 + AI 补充画面
   - Localization & Dub —— 字幕、配音、翻译
   - Podcast Repurpose —— 播客切片成视频 / 声波图
   - Screen Demo —— 软件录屏美化与走查
   - Talking Head —— 口播 / 访谈
2. **统一的八段流程**：`research → proposal → script → scene_plan → assets → edit → compose`，每段配一份导演技能，agent 读完再调工具、自检、存检查点。
3. **研究是一等公民**：写脚本前先搜 YouTube / Reddit / Hacker News / 新闻站 / 学术源，产出带引用的研究简报，避免编造事实。
4. **Provider 打分选择**：每个环节在多个供应商之间按 7 个维度打分，并留下可审计的决策日志。
5. **制作治理**：质量闸门、预算控制（Budget Controls）、决策审计轨迹（Decision Audit Trail）。
6. **零密钥也能跑**：不配任何 API Key 时，仍可用免费素材源 + 本地 TTS（piper-tts）完成一条片子。
7. **自检闭环**：`ffprobe` 校验、抽帧检查、音频电平分析、交付承诺核对、字幕检查。

## 安装与使用

前置：Python 3.10+、FFmpeg、Node.js 18+、一个 AI 编码助手。

```bash
git clone https://github.com/calesthio/OpenMontage.git
cd OpenMontage
make setup
```

没有 `make` 时（macOS/Linux）：

```bash
python3 -m venv .venv && source .venv/bin/activate
python -m pip install -r requirements.txt
cd remotion-composer && npm install && cd ..
python -m pip install piper-tts
cp .env.example .env
```

在编码助手打开项目后直接下指令：

```
"Make a 60-second animated explainer about how neural networks learn"
"Make a 75-second documentary montage about city life in the rain. Use real footage only, no narration, elegiac tone, with music."
```

`.env` 里每个 Key 都是可选的，有什么配什么（图像/视频网关、Kling、免费素材站、音乐、语音、更多视频供应商）。

## 使用场景

- **需要「一条完整片子」而不只是碎片素材**：研究、脚本、配音、配乐、字幕、渲染一次走完。
- **零预算试水**：不配 Key 走免费素材 + 本地 TTS 路线。
- **长内容再分发**：Clip Factory / Podcast Repurpose 把一次生产拆成多条短视频。
- **软件产品演示**：Screen Demo 生产线专做录屏美化与走查。

## 优势与特点

- **流程可检查**：阶段化 + 检查点 + 人工确认，比「一次性生成」更接近真实制片协作。
- **决策可审计**：供应商打分 7 维、预算控制、决策日志都留在库里。
- **素材合法**：纪录片线只用开放素材库，规避版权风险。
- **不锁定供应商**：图像/视频/语音/音乐均可换。
- **规模足够大**：700+ 技能文件意味着大量制作经验被写成可复用指令。

## 注意事项

- **AGPL-3.0**：自用出片没问题；若把代码并进对外提供的服务，会触发 AGPL 的源码开放义务，商用集成前要先评估。
- **体积重**：12 条生产线 + Remotion 合成层 + Python 依赖，安装与环境维护成本明显高于单一 skill；只为做一支 60 秒产品片，属于「用整套工厂造一颗螺丝」。
- **中文场景未做专项优化**：字幕断句、配音音色、中文排版需自己补，中文口播类需求可对比 `chengfeng-videocut-skills`。
- **依赖外部 API 才有生成画面**：不配 Key 时画面来自免费实拍素材库，做不了纯 AI 生成的视觉风格。
- **星数增长异常快**（2026-03 建仓，半年 69.8K），选型时应以实际跑通一条生产线为准，不要只看星数。

## 标签

`#AI大模型` `#代码开发工具` `#视频生成与剪辑` `#Agent工作流` `#Remotion` `#开源制片系统` `#AGPL`
