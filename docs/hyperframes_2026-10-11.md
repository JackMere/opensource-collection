# HyperFrames

> 收录时间：2026-10-11 | 最近核验：2026-10-19 | 原始链接：https://github.com/heygen-com/hyperframes | ⭐ 67.3K | 🍴 6.1K

## 基础信息

| 项 | 内容 |
|----|------|
| 作者 | heygen-com（HeyGen） |
| 开源协议 | Apache-2.0（无商用门槛、无按次渲染费） |
| 技术栈 | TypeScript、JavaScript（组合本身是纯 HTML/CSS） |
| 所属领域 | AI大模型、代码开发工具 |
| 核心用途 | 视频生成与剪辑 |
| 最近更新 | 2026-10-19 |
| 创建时间 | 2026-03-10 |
| 文档站点 | https://hyperframes.dev |

## 项目简介

HeyGen 开源的确定性视频渲染框架，口号 "Write HTML. Render video. Built for agents."：把 HTML、CSS、媒体与可寻址（seekable）动画，逐帧渲染成确定性 MP4。同一份输入永远产出同一支片，正好抵消 AI 生成视频的随机性。

它的赌注与 Remotion 不同：**Remotion 赌 React 组件，HyperFrames 赌人类和 agent 都能直接写的纯 HTML**。官方文档明确承认受 Remotion 启发，两者都用 headless Chrome + FFmpeg 渲染，差别在授权模型与编写模型。

## 核心功能

1. **HTML 原生组合**：`data-*` 时间属性 + `class="clip"` + 轨道/子组合，没有专有格式，也没有 React 依赖。
2. **自带 21 个 agent skills**，按需加载，核心是一个 `/hyperframes` 路由 + 领域技能：
   - `/product-launch-video` —— **产品宣传片**：给一个官网 URL、一份 brief 或脚本，产出 30–90 秒（上限约 3 分钟）的宣传/站点导览片
   - `/pr-to-video` —— 从 GitHub PR 生成更新说明片
   - `/faceless-explainer`、`/motion-graphics`、`/music-to-video`、`/slideshow`、`/talking-head-recut`、`/embedded-captions`、`/general-video`
   - `/remotion-to-hyperframes` —— **把现有 Remotion 工程单向迁移过来**
3. **CLI 完整开发闭环**：`init` / `lint` / `check` / `snapshot` / `preview` / `render` / `publish` / `doctor`。
4. **三条渲染路线**：本地、Docker、云（HeyGen 托管 `cloud render` / AWS Lambda `lambda deploy`）。
5. **动效运行时自由**：GSAP、Lottie、Three.js、Anime.js、CSS、WAAPI、TypeGPU 都能接。
6. **设计语言转译层 `frame.md`**：把网站的 design token 改写成适合视频画幅的规范（含 `design.md`），避免 agent 直接套网页样式导致尺度失真。
7. **素材库**：`npx hyperframes add` 可装现成组件（闪白转场、社媒关注动效、数据图表等）。

## 安装与使用

前置：**Node.js 22+、FFmpeg**（本机实测 node 22.23.1 + ffmpeg 6.1.1 均满足）。

```bash
# 建工程 → 预览 → 出片
npx hyperframes init my-video
npx hyperframes preview     # 浏览器实时预览
npx hyperframes render      # 渲染 MP4

# 装 agent skills（Claude Code 插件形态）
claude plugin marketplace add heygen-com/hyperframes
claude plugin install hyperframes@hyperframes

# 或独立 skills（Codex / OpenCode / 其他）
npx skills add heygen-com/hyperframes --skill product-launch-video
npx hyperframes skills update          # 从 main 装整套 core
```

典型指令：`/hyperframes` 下说「做一个 10 秒产品开场，淡入标题 + 背景视频 + 轻背景音乐」。

## 使用场景

- **产品宣传片 / 官网导览片**：`/product-launch-video` 直接吃官网 URL，抓真实页面视觉。
- **版本更新片**：`/pr-to-video` 把 PR 变成 changelog 视频。
- **动效短片与 Logo 演绎**：`/motion-graphics`，10 秒以内、无需旁白。
- **同一套前端组件复用**：页面与视频共用 design token，改品牌色不用重做片子。

## 优势与特点

- **Apache-2.0**：无按次渲染费、无商用门槛，这是相对 Remotion 最实际的差别（Remotion 超过 3 人的营利公司需买 Company License）。
- **对 agent 更友好**：纯 HTML 比 React 组件树更容易一次写对，这是它 21 个 skill 能跑通的前提。
- **官方直接给产品宣传片工作流**，不是社区拼装；同门还有 `hyperframes-launches` 仓开源了 HeyGen 自己的发布片工程，可直接改。
- **工程可迁移**：有 `/remotion-to-hyperframes` 单向迁移路径，选型不至于被锁死。

## 注意事项

- **渲染慢**：第三方同 brief 实测 16 个 skill，HyperFrames 用时 **23.5 分钟**，Remotion 官方 skills 为 **4.5 分钟**，差约 5 倍。原因是每帧都要在 headless Chrome 里 seek，HTML 组合图层更多。片长超过 90 秒时这个差距会变成「当天能不能出片」的问题。
- **吃内存**：1080p 长片逐帧渲染峰值 1–1.5G，4G 内存机器上要错峰跑。
- **没有原生音频生成**：配乐/音效靠外部素材或生成模型，`/music-to-video` 只是按节拍对齐已有音轨。
- **中文排版无专项优化**：字体、断句、字幕需要自己补规则。
- **星数增长异常快**（2026-03 建仓，7 个月 67K），选型应以跑通一条真实片为准。

## 更新记录

- **2026-10-19**：星数 60.6K → **67.3K**（fork 5.4K → 6.1K）；补充官方 21 个 skills 明细（含 `/product-launch-video`、`/pr-to-video`、`/remotion-to-hyperframes`）、三条渲染路线、`frame.md` 设计语言转译层，以及第三方实测的渲染耗时对比。

## 标签

`#AI大模型` `#代码开发工具` `#视频生成与剪辑` `#HTML渲染` `#Agent Skills` `#Apache-2.0` `#产品宣传片`
