# Karpathy "让 AI 输出更好懂"推文调研

调研日期：2026-10-05。星数为当天 GitHub 搜索结果。

## 一、推文

- 作者：Andrej Karpathy
- 时间：2026-10-02
- 链接：<https://x.com/karpathy/status/2105819303471976479>

**核心判断：** 模型能独立完成的活越来越多，人的工作会往上移，变成**监督和理解**模型的产出。所以，让模型用容易理解的形式交付结果很重要。好在代码和智能都越来越便宜，可以为理解一件事，临时做一个以前不值得做、**用完就扔**的产物。

## 二、四个部分

他按"后一种更好"的顺序给了四种格式：

| # | 格式 | 怎么用 | 要点 |
|---|---|---|---|
| 1 | **ASD-STE100 文字** | 让模型"用 ASD-STE100 解释" | 航空维修手册用的受控英语。模型很熟悉它，严格的写法规则让输出好读很多。规范太严，他常要求"做到 80%" |
| 2 | **图** | 让模型画图，不写字 | 图更容易处理、解析和理解 |
| 3 | **网页** | 让模型"用 HTML 输出" | 得到漂亮、可交互的网页。模型做前端越来越强，能做带动画的交互页面 |
| 4 | **讲解视频** | "做一个 3b1b 风格的讲解视频" | 他最看好的格式：任意主题的定制讲解视频。配音用 ElevenLabs API key，或让模型找本地运行的免费替代 |

**ASD-STE100 是什么：** 欧洲航空航天与防务工业协会（ASD）1986 年起维护的受控英语标准。起因是维修手册要给各国技师看，读错一条指令可能出人命。当前版本 Issue 9（2025 年 1 月），9 节 53 条写作规则，约 900 个受控词。典型规则：

- 一句一个动作
- 操作句 ≤20 词，说明句 ≤25 词
- 主动语态
- 不用分号
- 名词堆叠 ≤3 个
- 一个词只有一个意思
- 不用完成时

**注意：** 推文配的那张 ASD-STE100 总览图有错，见 [sources.md](../skills/karpathy-output-ladder/references/sources.md#推文配图的一处错误)。

## 三、GitHub 上的相关项目

**结论：** 能完整对应这四个部分的项目，都是推文后两天内出现的，星数还很低；高星项目都只覆盖其中一级。

### 完整覆盖四级

| 项目 | Stars | 特点 |
|---|---|---|
| [bowen31337/comprehension-ladder](https://github.com/bowen31337/comprehension-ladder) | 0 | 最完整：严格度可调、STE linter、SVG 图纸风格、HTML 模板、Manim 视频流水线 |
| [ashryaagr/karpathy-output-style](https://github.com/ashryaagr/karpathy-output-style) | 15 | 拆成四个小 skill；对推文配图做了勘误 |
| [ohernandezdev/karpathy-output](https://github.com/ohernandezdev/karpathy-output) | 4 | 强调"产物是视图，不是证据"，附待核对清单 |

### 第 1 部分：ASD-STE100

| 项目 | Stars | 特点 |
|---|---|---|
| [AminBlg/SimpleEnglish](https://github.com/AminBlg/SimpleEnglish) | ~3.8k | 文档和聊天回复分开定规则；有 lint hook 和评测 |
| [danyuchn/asd-ste100-skill](https://github.com/danyuchn/asd-ste100-skill) | ~3.7k | Strict / STE-flavored 两档；英文 linter；"情态是内容" |
| [0xpili/simplified-technical-english](https://github.com/0xpili/simplified-technical-english) | ~870 | 规则 + 词表 + 检查工具 |
| [lemonhall/asd-ste100-skill-zh](https://github.com/lemonhall/asd-ste100-skill-zh) | 7 | **中文版**：按中文的毛病重写规则（虚动词、套话、同义轮换、翻译腔），带中文 linter |

### 第 2 部分：图

| 项目 | Stars | 特点 |
|---|---|---|
| [tt-a1i/archify](https://github.com/tt-a1i/archify) | ~78k | 架构、流程、时序、数据流、状态图，输出可交互 HTML，可导出图片 |
| [coleam00/excalidraw-diagram-skill](https://github.com/coleam00/excalidraw-diagram-skill) | ~4.9k | Excalidraw 手绘风格 |
| [axtonliu/axton-obsidian-visual-skills](https://github.com/axtonliu/axton-obsidian-visual-skills) | ~3.6k | Obsidian Canvas / Excalidraw / Mermaid |

### 第 3 部分：交互 HTML

| 项目 | Stars | 特点 |
|---|---|---|
| [alexanderop/walkthrough](https://github.com/alexanderop/walkthrough) | ~140 | 代码库讲解页，带可点击的 Mermaid 图 |
| [ds-vibe/html-explainer](https://github.com/ds-vibe/html-explainer) | ~17 | 交互讲解页，带自动 QA |

### 第 4 部分：讲解视频

| 项目 | Stars | 特点 |
|---|---|---|
| [3b1b/manim](https://github.com/3b1b/manim) | ~95k | 3Blue1Brown 本人的动画引擎 |
| [ManimCommunity/manim](https://github.com/ManimCommunity/manim) | ~41k | 社区维护版，文档全，更适合让模型写 |
| [iart-ai/motion-skills](https://github.com/iart-ai/motion-skills) | ~700 | 动效/视频 skill 合集，含 Manim |

## 四、怎么结合

成品：[`skills/karpathy-output-ladder/`](../skills/karpathy-output-ladder/SKILL.md)

**骨架**用 comprehension-ladder 的四级结构，四个步骤：

1. **选级**：用户点名就用，没点名就按内容选并说明理由。
2. **先写提纲**：核心结论、机制、情态、一个例子。四级产物都从同一份提纲出来，换形式不换事实。
3. **做产物**：每级一份参考文档。
4. **交付**：附待核对清单，再给出"下一步可选更高的级"。

**每一级取最强的来源：**

| 级 | 取自 |
|---|---|
| 1 文字 | danyuchn（结构/词汇规则分层、情态是内容、英文 linter）+ lemonhall（中文规则和中文 linter）+ SimpleEnglish（条件在前） |
| 2 图 | comprehension-ladder 的 SVG 图纸风格 + Mermaid 选型；需要更精致时指向 archify |
| 3 HTML | comprehension-ladder 的交互模式 + 自写的单文件模板（梯度下降演示，真实计算） |
| 4 视频 | Manim Community + ElevenLabs/本地 TTS + ffmpeg；按拍对齐的做法来自 comprehension-ladder |

**贯穿四级的两条铁律**（几个项目的共识）：

- **不编造**：图的箭头、网页的默认值、视频的过渡句里，不补原文没有的原因和数字。
- **保留情态**：原文的"可能"，在文字、图（虚线）、网页（说明文字）、视频（旁白）里都要保留。

**加上 ohernandezdev 的证据规则：** 产物是看原文的视图，不是证据，交付时附待核对清单。

**针对中文用户的调整：**

- 1 级默认按受控中文写：说明句 ≤40 字，操作句 ≤25 字。
- 图和视频的字体加了中文字体。
- 旁白规则里加了"符号写成读音"。

**刻意没做的：**

- **没有自带完整视频渲染脚本。** Manim 依赖系统里的 Cairo 和 Pango，环境差异大。参考文档给出每一步的命令；装不上时退而交付分镜表，不假装做出了视频。
- **没有复制 ASD 的受控词表。** 标准不允许再分发。

## 五、验证情况

- 两个 linter：自检通过；用参考文档里的正反例跑过，结果和预期一致。
- HTML 模板：在 Chromium 里测过浅色和深色主题、375px 手机宽度。没有控制台报错，滑块、上一步/下一步、预设按钮都会更新说明文字，数值和手算一致。
- 两个 Mermaid 例子：用 Mermaid 11 实际渲染通过。
- 视频：ffmpeg 部分（每拍时长、拼接、合成、抽帧）用合成音视频跑通。Manim 场景只做了语法检查；本环境缺 Pango，没能实际渲染。
