<sub>🌐 <b>中文</b> · <a href="README.en.md">English</a></sub>

<div align="center">

# Huashu Design Lite

> *「打字。回车。一份能交付的设计。」*
>
> *"Type. Hit enter. A finished design lands in your lap."*

[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Agent-Agnostic](https://img.shields.io/badge/Agent-Agnostic-blueviolet)](https://skills.sh)
[![Skills](https://img.shields.io/badge/skills.sh-Compatible-green)](https://skills.sh)

<br>

**在你的 agent 里打一句话，拿回一份能交付的设计。**

3 到 30 分钟，你能 ship 一段**产品发布动画**、一个能点击的 App 原型、一套能编辑的 PPT、一份印刷级的信息图。不是「AI 做的还行」那种水平——是看起来像大厂设计团队做的。给 skill 你的品牌资产（logo、色板、UI 截图），它会读懂你的品牌气质；什么都不给，**按设计复杂度自动选模式**：普通新设计出 2 个方向，高不确定性任务才跑完整三方向顾问 + 60 种 HTML 原生风格库，兜底到不出 AI slop。

**你看到这篇 README 里的每一个动画，都是 huashu-design 自己做的（这些能力本 fork 全部保留）。** 不是 Figma，不是 AE，就是一句话 prompt + skill 跑通。

<br>

> **Huashu Design 的自适应轻量工作流版本。**
> 基于 [alchaincyf/huashu-design](https://github.com/alchaincyf/huashu-design)，保留其 HTML-native 设计能力、品牌资产协议、视觉探索、PPT / 动画 / 原型工具链和反 AI-slop 规则。

主要变化：

- **FAST**：局部修改直接执行，不生成无意义的设计方向
- **STANDARD**：普通新设计默认 2 个方向 + compact spec
- **FULL**：高价值、高不确定性设计保留原版完整三方向流程
- references 按需加载，降低上下文消耗
- runtime 按实际能力降级，而不是按模型厂商判断
- 明确参考或已批准方向时，可直接进入实现

核心原则：

> Workflow complexity should scale with design uncertainty,
> not task existence.

```
npx skills add Soonkeira/huashu-design-lite
```

跨 agent 通用——Claude Code、Cursor、Codex、OpenClaw、Hermes 都能装。

> 📣 **本仓库是 fork，不是上游。** 想做完整三方向、要原版全量表装 [alchaincyf/huashu-design](https://github.com/alchaincyf/huashu-design)；想让工作流随设计复杂度伸缩，装这个。

> 📣 **MIT 协议。** 上游自 2026-05-14 起完全开源（[MIT License](LICENSE)），个人和**商用都免费**，本 fork 沿用同一协议。([查看变更](#license))

[设计复杂度路由](#设计复杂度路由) · [安装](#装上就能用) · [能做什么](#能做什么) · [核心机制](#核心机制) · [和 Claude Design 的关系](#和-claude-design-的关系)

</div>

---

<p align="center">
  <img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/hero-animation-v10-en.gif" alt="huashu-design Hero · 打字 → 选方向 → 画廊展开 → 聚焦 → 品牌显形" width="100%">
</p>

<p align="center"><sub>
  ▲ 25 秒 · Terminal → 4 方向 → Gallery ripple → 4 次 Focus → Brand reveal<br>
  👉 <a href="https://www.huasheng.ai/huashu-design-hero/">访问带音效的 HTML 互动版</a> ·
  <a href="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/hero-animation-v10-en.mp4">下载 MP4（含 BGM+SFX · 10MB）</a>
</sub></p>

---

## 📺 新手教程（花叔亲录）

不知道怎么用？看上游作者花叔录的 huashu-design 上手教程（讲的是通用能力，本 fork 的差异见 [设计复杂度路由](#设计复杂度路由)）：

<p align="center">
  <a href="https://www.youtube.com/watch?v=m-_BlUdcIvw"><img src="https://img.youtube.com/vi/m-_BlUdcIvw/maxresdefault.jpg" alt="huashu-design 使用教程" width="70%"></a>
</p>

<p align="center"><sub>👉 <a href="https://www.youtube.com/watch?v=m-_BlUdcIvw">在 YouTube 观看完整教程</a></sub></p>

---

## 装上就能用

```bash
npx skills add Soonkeira/huashu-design-lite
```

> **装完先自检**：这个 skill 不只是 SKILL.md 一个文件，`references/`、`assets/`、`scripts/`、`demos/` 四个子目录里有 99 处被引用的配方、脚本、素材，缺一不可。装完看一眼安装目录（如 `~/.claude/skills/huashu-design-lite/`），如果只有 SKILL.md、没有那几个子目录，说明你的 `skills` CLI 版本太旧（≤1.5.15 有个只同步单文件的 bug，已在 1.5.19 修复）。升级后再装一次即可：
>
> ```bash
> npm i -g skills@latest        # 或 npx skills@latest add Soonkeira/huashu-design-lite
> ```
>
> 升级后仍异常，就用 `git clone` 兜底安装，把仓库克隆到任意 skills 目录即可：
>
> ```bash
> git clone https://github.com/Soonkeira/huashu-design-lite.git ~/.claude/skills/huashu-design-lite
> ```

然后在 Claude Code / Codex / Cursor 等任意支持 skills 的 agent 里直接说话：

```
「把这个博客文章页左右留白缩小一点，正文宽一些，移动端别溢出」   → FAST，直接改
「做一份 AI 心理学的演讲 PPT」                                  → STANDARD，2 个方向
「做个正式发布官网 + 30 秒 launch film，完整探索设计方向」        → FULL，完整三方向
「帮我对这个设计做一个 5 维度评审」                              → 只走评审流程
```

没有按钮、没有面板、没有 Figma 插件。

---

## 能做什么

| 能力 | 交付物 | 典型耗时 |
|------|--------|----------|
| 交互原型（App / Web） | 单文件 HTML · 真 iPhone bezel · 可点击 · Playwright 验证 | 10–15 min |
| 演讲幻灯片 | HTML deck（浏览器演讲）+ 可编辑 PPTX（文本框保留） | 15–25 min |
| 时间轴动画 | MP4（25fps / 60fps 插帧）+ GIF（palette 优化）+ BGM | 8–12 min |
| 设计变体 | 3+ 并排对比 · Tweaks 实时调参 · 跨维度探索 | 10 min |
| 信息图 / 可视化 | 印刷级排版 · 可导 PDF/PNG/SVG | 10 min |
| 方向探索（STANDARD / FULL） | 按复杂度出 2–3 版真实视觉：从三套互补逻辑（秒数轮盘 + 现实参照获奖站 + 最佳设计师）中取 2 套（STANDARD）或 3 套（FULL） | 5 min |
| 5 维度专家评审 | 雷达图 + Keep/Fix/Quick Wins · 可操作修复清单 | 3 min |

---

## 设计复杂度路由

收到设计任务后先判断模式。除非显式指定 `/design fast`、`/design standard`、`/design full`，否则自动判断。

| 模式 | 什么时候用它 | 行为 |
|---|---|---|
| **FAST** | 局部修改 · 调间距/字号/颜色/对齐 · 修 responsive / overflow / clipping · 已选定方向后的迭代 · 用户给了明确参考要求照做 · 机械性视觉任务 | 不进入三方向流程 · 不生成 design-demos · 不写长篇 spec · 没有真正阻塞项就不提澄清问题 · 直接实现，完成后至少检查 Desktop + Mobile |
| **STANDARD**（默认） | 新页面 · 普通 Landing Page · Homepage redesign · Dashboard · App 页面/原型 · 普通信息图——有 design context，但视觉方向仍需选择 | 最多问 2 个真正影响设计的问题 · 默认出 **2 个明显不同的真实方向** · 已有明确 reference / 品牌系统 / approved direction 时直接做 1 个主方案，不凑数制造变体 |
| **FULL** | 品牌从零设计 · 高价值正式官网 · 正式产品发布页 · Launch Film / Campaign · 正式对外 PPT / Pitch Deck · 需求高度模糊 · 明确要求完整探索 | 保留原版完整工作流：完整事实与品牌资产验证 → 三方向真实视觉 → 完整 design spec → 用户选择 Gate → 多轮视觉验证 |

**显式覆盖优先于自动判断**：「直接做，不用出方向」→ FAST；「给我几个方向看看」→ STANDARD 或 FULL；「完整探索后再做」→ FULL。不要因为任务属于「设计」就自动升级到 FULL。

> 只有**存在值得用户决策的设计分歧**时，才生成多个方向。

---

## Demo 画廊

### 方向探索（FULL 模式）

高价值或高不确定性任务的完整流程：**三套互补逻辑并行**——秒数轮盘（20 选 1 打破惯性）+ 现实参照（世界级获奖网站迁移）+ 最佳设计师（顶级工作室哲学），直接出 3 版**真实视觉**让你看着选，不让你在文字里盲选风格。背后是 **60 种 HTML 原生风格库**（网页 20 + PPT 20 + 信息图 20，纯 CSS 无需生图）。

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/w3-fallback-advisor.gif" width="100%"></p>

### iOS App 原型

iPhone 15 Pro 精确机身（灵动岛 / 状态栏 / Home Indicator）· 状态驱动多屏切换 · 真图从 Wikimedia/Met/Unsplash 取 · Playwright 自动点击测试。

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c1-ios-prototype.gif" width="100%"></p>

### Motion Design 引擎

Stage + Sprite 时间片段模型 · `useTime` / `useSprite` / `interpolate` / `Easing` 四 API 覆盖所有动画需求 · 一条命令导出 MP4 / GIF / 60fps 插帧 / 带 BGM 的成片。

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c3-motion-design.gif" width="100%"></p>

### HTML Slides → 可编辑 PPTX

HTML deck 浏览器演讲 · `html2pptx.js` 读 DOM 的 computedStyle 逐元素翻译成 PowerPoint 对象 · 导出的是**真文本框**，PPT 里双击即可编辑。

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c2-slides-pptx.gif" width="100%"></p>

### Tweaks · 实时变体切换

配色 / 字型 / 信息密度等参数化 · 侧边面板切换 · 纯前端 + `localStorage` 持久化 · 刷新不丢。

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c4-tweaks.gif" width="100%"></p>

### 信息图 / 数据可视化

杂志级排版 · CSS Grid 精准分栏 · `text-wrap: pretty` 排印细节 · 真数据驱动 · 可导 PDF 矢量 / PNG 300dpi / SVG。

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c5-infographic.gif" width="100%"></p>

### 5 维度专家评审

哲学一致性 · 视觉层级 · 细节执行 · 功能性 · 创新性 各 0–10 分 · 雷达图可视化 · 输出 Keep / Fix / Quick Wins 清单。

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/c6-expert-review.gif" width="100%"></p>

### Junior Designer 工作流

不闷头做大招：先写 assumptions + placeholders + reasoning，尽早 show 给你，再迭代。理解错了早改比晚改便宜 100 倍。

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/w2-junior-designer.gif" width="100%"></p>

### 品牌资产协议 5 步硬流程

涉及具体品牌时强制执行：问 → 搜 → 下载（三条兜底）→ grep 色值 → 写 `brand-spec.md`。

<p align="center"><img src="https://github.com/alchaincyf/huashu-design/releases/download/v2.0/w1-brand-protocol.gif" width="100%"></p>

---

## Showcase · 真实案例

### 鹦鹉进化史网站 · FULL 模式三方向实战（上游案例）

> **Live demo · [https://www.huasheng.ai/parrots/](https://www.huasheng.ai/parrots/)**

一句「做个介绍鹦鹉进化史的网站」、零额外要求，skill 走 FULL 模式跑完整顾问流程：先判断图片是内容必需 → 抓公共领域博物插画（Edward Lear / John Gould 的鹦鹉图录）→ **三套逻辑并行**（秒数轮盘 + 现实参照获奖站 + 原研哉「白」哲学）各出一版真实视觉。**素材齐了再设计，不是边设计边用色块占位。**

### 「聊聊 skill」 · PM after-party 演讲 deck

> **Live demo · [https://skill-huasheng.vercel.app](https://skill-huasheng.vercel.app)**

13 页 HTML deck，**全部用 huashu-design 完成**：

- 黑底极简衬线视觉系统（cover / about / hook / what / why / closing）
- 2 个带 BGM + SFX 的 22 秒 cinematic demo（Nuwa skill workflow + Darwin skill workflow），各采用**完全独立的视觉语言**：
  - **Nuwa**：3D 知识 orbit + Pentagon 提炼 + SKILL.md typewriter + 「21 分钟」hero reveal
  - **Darwin**：autoresearch loop spin + v1/v5 并列 diff + Hill-Climb 全屏曲线 + Ratchet gear lock
- 每个 cinematic 默认显示**完整静态 workflow dashboard**（观众随时能看清 skill 怎么跑），点 ▶ 才触发动画，跑完自动 fade 回 dashboard
- 嵌入 huasheng.ai 的 25 秒 hero 动画（iframe 本地化兜底）
- 真实数据：14,495 stargazers 真实曲线（gh API 拉取）+ DeepSeek V4 真实 specs（WebSearch 验证）
- 真实 AI 素材：用 `huashu-gpt-image` 跑 4×2 grid 大图，`extract_grid.py` 抠出 8 张独立透明 PNG，做 3D orbit 漂浮

**适合参考的页面**：
- `/slides/slide-04b-nuwa-flow.html` · 静态 dashboard + cinematic overlay 双层架构
- `/slides/slide-06b-darwin-flow.html` · 完全独立视觉语言的对照案例
- `/slides/slide-03b-deepseek-cover.html` · AI slop vs 真实设计师视角的对比页

详细 cinematic patterns 见 `references/cinematic-patterns.md`。

---

## 核心机制

### 品牌资产协议

skill 里最硬的一段规则。涉及具体品牌（Stripe、Linear、Anthropic、自家公司等）时强制执行 5 步：

| 步骤 | 动作 | 目的 |
|------|------|------|
| 1 · 问 | 用户有 brand guidelines 吗？ | 尊重已有资源 |
| 2 · 搜官方品牌页 | `<brand>.com/brand` · `brand.<brand>.com` · `<brand>.com/press` | 抓权威色值 |
| 3 · 下载资产 | SVG 文件 → 官网 HTML 全文 → 产品截图取色 | 三条兜底，前一条失败立刻走下一条 |
| 4 · grep 提取色值 | 从资产里抓所有 `#xxxxxx`，按频率排序，过滤黑白灰 | **绝不从记忆猜品牌色** |
| 5 · 固化 spec | 写 `brand-spec.md` + CSS 变量，所有 HTML 引用 `var(--brand-*)` | 不固化就会忘 |

A/B 测试（v1 vs v2，各跑 6 agent）：**v2 的稳定性方差比 v1 低 5 倍**。稳定性的稳定性，这是 skill 真正的护城河。

### 设计方向顾问（按模式触发）

不是所有视觉任务的必经步骤，而是用来解决「设计方向存在真实不确定性」：

- **FAST**：完全跳过本节流程，直接沿现有 design context 改，不创建无意义的视觉变体
- **STANDARD**：视觉方向未定才触发，默认 2 个明显不同的真实视觉；已有明确 reference / 品牌系统 / 现有设计语言 / approved direction 则跳过方向探索直接实现；不允许为了满足流程而制造只有配色不同的假变体
- **FULL**：执行完整流程——先对话澄清 + 主动索要参考（名字 / logo / 品牌色 / 喜欢的参考站）→ 取齐内容必需的真图（公共领域 / 免版权，脚本一键抓）→ **多套互补逻辑并行 subagent**，各出一版**真实视觉**：① 秒数轮盘（`date +%S` 取秒，20 选 1，打破模型偷选极简的惯性）② 现实参照（世界级获奖网站 / PPT / iOS 原型迁移）③ 最佳设计师（预算无上限时最适合的工作室哲学）
- **绝不让你在没看到视觉时盲选风格**——方向摆出来，看着选
- 以下情况直接跳过方向探索：用户明说「直接做」「不用出方案」· 已选定方向后的迭代 · 局部视觉修改 · bug / responsive / overflow 修复 · 纯文字/导出/截图等机械操作 · 用户提供了足够明确的目标稿并要求忠实实现

底层是 **60 种 HTML 原生风格库**（网页 20 + PPT 20 + 信息图 20，按大胆 / 中性 / 安静分级，纯 CSS 无需生图）作弹药，不是教条。references 一律按需加载——只读当前步骤真正需要的文件，禁止为了「完整理解 skill」一次性读完。

### Junior Designer 工作流

默认工作模式，贯穿所有任务：

- 开工前 show 问题清单一次性发给用户，等批量答完再动手
- HTML 里先写 assumptions + placeholders + reasoning comments
- 尽早 show 给用户（哪怕只是灰色方块）
- 填充实际内容 → variations → Tweaks 这三步分别再 show 一次
- 交付前用 Playwright 肉眼过一遍浏览器

### 反 AI slop 规则

避免一眼 AI 的视觉最大公约数（紫渐变 / emoji 图标 / 圆角+左 border accent / SVG 画人脸 / Inter 做 display）。用 `text-wrap: pretty` + CSS Grid + 精心选择的 serif display 和 oklch 色彩。

---

## 和 Claude Design 的关系

我大方承认：品牌资产协议的哲学是从 Claude Design 流传出来的提示词里偷师的。那份提示词反复强调**好的高保真设计不是从白纸开始，而是从已有的设计上下文长出来**。这个原则是 65 分作品和 90 分作品的分水岭。

定位差异：

| | Claude Design | huashu-design-lite |
|---|---|---|
| 形态 | 网页产品（浏览器里用） | skill（Claude Code 里用） |
| 配额 | 订阅 quota | API 消耗 · 并行跑 agent 不受 quota 限 |
| 交付物 | 画布内 + 可导 Figma | HTML / MP4 / GIF / 可编辑 PPTX / PDF |
| 操作方式 | GUI（点、拖、改） | 对话（说话、等 agent 做完） |
| 复杂动画 | 有限 | Stage + Sprite 时间轴 · 60fps 导出 |
| 跨 agent | 专属 Claude.ai | 任意 skill 兼容 agent |

Claude Design 是**更好的图形工具**，huashu-design-lite 是**让图形工具这层消失**。两条路，不同受众。

---

## 安全与数据流

核心链路（设计→渲染→MP4/PDF/PPTX导出）**100%本地运行，零网络零key**。云能力（豆包TTS配音、AI看片评审）全部隔离在 `scripts/cloud/`，完全可选：用你自己的key、只发对应厂商官方API、首次调用需 `--yes` 显式确认。无telemetry，没有任何数据发往作者服务器。全部出站域名、密钥处理、删除边界的穷举声明见 [SECURITY.md](SECURITY.md)，欢迎用你的agent对着代码逐条核验。

---

## Limitations

- **不支持图层级可编辑的 PPTX 到 Figma**。产出 HTML，可截图、录屏、导图，但不能拖进 Keynote 改文字位置。
- **Framer Motion 级别的复杂动画不行**。3D、物理模拟、粒子系统超出 skill 边界。
- **完全空白的品牌从零设计质量会掉到 60–65 分**。凭空画 hi-fi 本来就是 last resort。

这是一个 80 分的 skill，不是 100 分的产品。对不愿意打开图形界面的人，80 分的 skill 比 100 分的产品好用。

---

## 仓库结构

```
huashu-design-lite/
├── SKILL.md                 # 主文档（给 agent 读）
├── README.md                # 中文 README（默认，本文件）
├── README.en.md             # 英文 README
├── assets/                  # Starter Components
│   ├── animations.jsx       # Stage + Sprite + Easing + interpolate
│   ├── ios_frame.jsx        # iPhone 15 Pro bezel
│   ├── android_frame.jsx
│   ├── macos_window.jsx
│   ├── browser_window.jsx
│   ├── deck_stage.js        # HTML 幻灯片引擎
│   ├── deck_index.html      # 多文件 deck 拼接器
│   ├── design_canvas.jsx    # 并排变体展示
│   ├── showcases/           # 24 个预制样例（8 场景 × 3 风格）
│   └── bgm-*.mp3            # 6 首场景化背景音乐
├── references/              # 按任务深入读的子文档
│   ├── animation-pitfalls.md
│   ├── design-styles.md     # 60 种 HTML 原生风格库（网页 20 + PPT 20 + 信息图 20）
│   ├── slide-decks.md
│   ├── editable-pptx.md
│   ├── critique-guide.md
│   ├── video-export.md
│   └── ...
├── scripts/                 # 导出工具链
│   ├── render-video.js      # HTML → MP4
│   ├── convert-formats.sh   # MP4 → 60fps + GIF
│   ├── add-music.sh         # MP4 + BGM
│   ├── export_deck_pdf.mjs
│   ├── export_deck_pptx.mjs
│   ├── html2pptx.js
│   └── verify.py
└── demos/                   # 9 个能力演示 (c*/w*)，中英双版 GIF/MP4/HTML + hero v10
```

---

## 上游与来源

本仓库 fork 自 [alchaincyf/huashu-design](https://github.com/alchaincyf/huashu-design)，原作者花叔（花生）。上游的 HTML-native 设计引擎、品牌资产协议、工具链与全部视觉资产都归上游所有，本 fork 只改动了工作流的复杂度路由（详见 [设计复杂度路由](#设计复杂度路由)）。下面的起源故事属于上游。

Anthropic 发布 Claude Design 那天我玩到凌晨四点。几天之后发现自己再也没点开过它，不是它不好——它是这个赛道目前最成熟的产品——是我宁愿让 agent 在终端里帮我干活，也不愿意打开任何图形界面。

于是让 agent 拆解 Claude Design 本身（包括社区流传的系统提示词、品牌资产协议、组件机制），蒸馏成结构化 spec，再写成 skill 装进自己的 Claude Code。

感谢 Anthropic 把 Claude Design 的提示词写得清晰。这种基于其他产品灵感的二次创作，是开源文化在 AI 时代的新形态。

---

## 上游 · 用 huashu-design 做的产品

**[FanBox · Coding Agent 的驾驶舱](https://github.com/alchaincyf/fanbox)** 的三套界面皮肤，就是用 huashu-design 设计的。指挥 Claude Code / Codex 干活，看清它碰过的每个文件、每一行改动。

[![FanBox · Coding Agent 的驾驶舱](https://raw.githubusercontent.com/alchaincyf/fanbox/master/assets/promo-banner.jpg)](https://github.com/alchaincyf/fanbox)

---

## 上游 · 社区翻译版本

社区维护的翻译版本。翻译质量与各版本 license 条款由对应维护者负责，使用前请先确认。

| 语言 | 维护者 | 仓库 |
|---|---|---|
| English | [@namandhakad712](https://github.com/namandhakad712) | [namandhakad712/huashu-design-en](https://github.com/namandhakad712/huashu-design-en) |
| 한국어（韩语） | [@ktkarchive](https://github.com/ktkarchive) | [ktkarchive/ktk-design](https://github.com/ktkarchive/ktk-design) |
| Tiếng Việt（越南语） | [@letrquan](https://github.com/letrquan) | [letrquan/huashu-design](https://github.com/letrquan/huashu-design) |

想加你的语言？fork 仓库、翻译 `SKILL.md` + `README.md`，然后回这边开个 issue，我会把链接加进来。

---

## License

**2026-05-14 起改为 MIT 协议。** 此前版本采用「个人使用免费、企业商用需授权」的 Personal Use License，对商用做了限制——现在这层限制完全解除。

按 [MIT License](LICENSE)，你可以**自由使用、修改、分发**本 skill，**包括商业用途**——公司内部用、客户商单交付、做成付费产品对外卖，都没问题。无需事先授权、无需付费、无需打招呼。注明出处不强制，但欢迎。本 fork（huashu-design-lite）沿用同一 MIT 协议。

---

## 上游 · Connect 花生（花叔）

花生是 AI Native Coder、独立开发者、AI 自媒体博主。代表作：小猫补光灯（AppStore 付费榜 Top 1）、《一本书玩转 DeepSeek》、女娲 .skill（GitHub 12000+ star）。自媒体全平台 30 万+ 粉丝。

| 平台 | 账号 | 链接 |
|---|---|---|
| X / Twitter | @AlchainHust | https://x.com/AlchainHust |
| 公众号 | 花叔 | 微信搜索「花叔」 |
| B 站 | 花叔 | https://space.bilibili.com/14097567 |
| YouTube | 花叔 | https://www.youtube.com/@Alchain |
| 小红书 | 花叔 | https://www.xiaohongshu.com/user/profile/5abc6f17e8ac2b109179dfdf |
| 官网 | huasheng.ai | https://www.huasheng.ai/ |
| 开发者主页 | bookai.top | https://bookai.top |

合作咨询、自媒体约稿 → 以上任一平台私信花生即可。
