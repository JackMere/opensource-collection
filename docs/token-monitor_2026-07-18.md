# token-monitor
> 收录时间：2026-07-18 | 原始链接：https://github.com/Javis603/token-monitor | Star: 暂无 | Fork: 暂无

## 基础信息
| 项 | 内容 |
|----|------|
| 作者 | Javis603 |
| 开源协议 | MIT |
| 技术栈 | Electron、Node.js、JavaScript/TypeScript |
| 所属领域 | AI大模型、代码开发工具、桌面客户端 |
| 核心用途 | AI工具资费监控、Token使用统计、多设备同步桌面组件 |

## 核心功能
为各类AI编码工具打造的实时Token使用、成本、限额监控桌面组件，支持多设备同步：
- 支持20+主流AI工具：Claude Code、Codex、OpenCode、Hermes、OpenClaw、Cursor、Antigravity、Cline、Kimi、Qwen、Grok Build、GitHub Copilot、Pi、Zed、Kiro、DeepSeek、Minimax、Volcengine、Ollama等
- 实时Token跟踪，秒级更新使用数据
- 多设备实时同步，支持局域网、Cloudflare Worker部署同步后端
- 多维统计视图：按工具、设备、模型、会话分组展示
- 会话详情查看：每个prompt的Token拆分、工具使用、缓存命中率统计
- 成本统计：支持多货币（人民币/美元/港币/新台币）汇率自动更新
- 用量趋势与仪表盘：活动热力图、趋势图表、K线视图、连续使用天数统计
- AI工具限额检测：自动查询各平台账号剩余额度、周限额、余额、消费情况
- 高度可定制：主题切换（深色/浅色/自定义颜色）、悬浮气泡模式、托盘图标显示、全局快捷键唤起
- 隐私优先：所有代码、对话内容均保留在本地，不上传到第三方服务器
- 支持数据导出：CSV/JSON格式导出，可对接Grafana、Obsidian等工具
- 支持WSL子系统使用统计自动合并
- 支持iOS小组件（通过Widgy/Scriptable）、Discord状态显示

## 部署使用方式
### 方式1：直接下载安装包（推荐普通用户）
从 [GitHub Releases](https://github.com/Javis603/token-monitor/releases) 下载对应平台安装包：
- macOS（Apple Silicon）：.dmg 安装包，签名公证
- Windows 10/11：安装版/便携版 .exe
- Linux x64：.AppImage 包
安装后默认本地模式运行，自动检测并跟踪本机AI工具使用，无需额外配置。

### 方式2：多设备同步部署
1. 选择一台常开设备作为同步Hub：在设置中开启"Host hub on this device"，生成密钥和访问地址
2. 其他设备在设置中选择"Connect to a hub"，输入Hub地址和密钥即可
3. 也可自行部署Cloudflare Worker作为公共同步后端，一键部署按钮项目已提供
4. 无头设备（无GUI服务器）可运行npm包 `npm run agent` 作为采集端上报数据

### 方式3：源码编译
需要Node.js 22.13+环境：
```bash
git clone https://github.com/Javis603/token-monitor.git
cd token-monitor
npm install
npm run start # 本地开发运行
npm run dist:mac # 编译macOS安装包
npm run dist:win # 编译Windows安装包
npm run dist:linux # 编译Linux AppImage
```

## 适用场景
1. 高频使用AI编码工具的开发者，需要统一统计多工具、多设备的Token消耗和成本支出
2. 团队/多设备使用者，需要同步查看多台机器上的AI工具使用情况
3. 需要监控AI平台额度消耗，避免超额使用
4. AI使用数据量化统计，优化使用习惯、控制成本
5. 对接OpenClaw、Claude Code等工具的使用监控，可视化统计会话消耗

## 优劣总结
- **优势**：
  - 支持AI工具非常全面，几乎覆盖所有主流AI编码/Agent工具
  - 多设备同步能力强，支持多种部署模式
  - 统计维度丰富，从Token、成本、会话、趋势到限额全面覆盖
  - 隐私保护到位，本地优先，数据不上传
  - 跨平台支持完善，桌面三大系统全覆盖
  - 定制化程度高，UI和展示可灵活配置
  - 原生支持OpenClaw，可直接读取本地OpenClaw会话数据
- **劣势/注意事项**：
  - 基于Electron开发，包体积相对较大
  - 同步功能需要自行部署Hub或Cloudflare Worker，对普通用户有一定门槛
  - 部分AI工具的限额查询需要自行配置API密钥
  - 目前不支持手机端原生APP，iOS仅能通过脚本小组件实现
