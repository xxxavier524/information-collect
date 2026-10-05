# 来源与致谢

## Karpathy 原帖

2026-10-02，<https://x.com/karpathy/status/2105819303471976479>

开头的判断：我们会花越来越多的时间去理解模型的输出。然后按"后一种更好"的顺序给了四种格式。以下英文摘录转引自社区文章和项目（本环境无法直接访问 x.com）：

**1. 文字**

> Ask your LLM to explain something in ASD-STE100, it's a controlled language specification originally developed for aerospace maintenance documentation.

> Sometimes I've tried to soften it a bit e.g. ask for "80% of the way to ASD-STE100" because the spec is quite stringent.

**2. 图**

> But even better: Diagrams / images. Instead of writing, ask your LLM to create a diagram. These can be a lot easier to process, parse, and understand.

**3. 网页**

> But even better: Web pages. Ask for output "in HTML" to get a beautiful, interactive webpage.

**4. 讲解视频**

> But even better: Explainer videos. The output format I am most bullish on is fully custom / bespoke explainer videos generated on any arbitrary topic.

他给的示例是让模型做"3b1b style video explainer"，用他的 ElevenLabs API key 配音，或者让模型找能用本地算力的免费替代方案。

**收尾**

> As LLMs get better, they will do more and more of the legwork autonomously, and a lot more of our work will rise up the abstractions into oversight and understanding.

> Luckily, LLMs can help here too because as intelligence and code are increasingly abundant, you can ask for large, custom, discardable software artifacts that would never have made sense to create before.

### 推文配图的一处错误

推文配了一张 ASD-STE100 的总览图。[ashryaagr/karpathy-output-style](https://github.com/ashryaagr/karpathy-output-style) 对照 Issue 9 指出：图里把 "approximately" 标成不推荐、建议用 "about"，但 Issue 9 实际批准 APPROXIMATELY 作副词，ABOUT 是介词，意思是"关于"。图里还用了旧术语 "technical names""noun clusters"，Issue 9 改叫 "technical nouns""multi-word nouns"。以正式标准为准。

## 参考项目

星数为 2026-10-05 查询时的数字。

### 完整覆盖四级的（都是推文后两天内出现的，星数还很低）

| 项目 | Stars | 许可证 | 借鉴了什么 |
|---|---|---|---|
| [bowen31337/comprehension-ladder](https://github.com/bowen31337/comprehension-ladder) | 0 | MIT | 四级结构；先写提纲、四级共用；情态在每一级都要保留；SVG 总览图风格；HTML 交互模式；旁白按拍、用 durations.json 对齐画面 |
| [ohernandezdev/karpathy-output](https://github.com/ohernandezdev/karpathy-output) | 4 | MIT | "产物是视图，不是证据"；交付时附待核对清单；没渲染就不说视频做好了，退而交付分镜 |
| [ashryaagr/karpathy-output-style](https://github.com/ashryaagr/karpathy-output-style) | 15 | MIT | "80%"是风格要求，不是合规分数；推文配图的勘误 |

### 1 级：受控文字

| 项目 | Stars | 许可证 | 借鉴了什么 |
|---|---|---|---|
| [AminBlg/SimpleEnglish](https://github.com/AminBlg/SimpleEnglish) | ~3.8k | MIT | 条件在前；回复和文档分开要求 |
| [danyuchn/asd-ste100-skill](https://github.com/danyuchn/asd-ste100-skill) | ~3.7k | MIT | 结构规则可执行、词汇规则只当方向；情态是内容；英文 linter（`scripts/ste-lint.py`，原样取用） |
| [0xpili/simplified-technical-english](https://github.com/0xpili/simplified-technical-english) | ~870 | MIT | 参考 |
| [lemonhall/asd-ste100-skill-zh](https://github.com/lemonhall/asd-ste100-skill-zh) | 7 | MIT | 中文的六类坑；中文句长口径；中文 linter（`scripts/ste-lint-zh.py`，原样取用）；推文英文原文摘录 |

### 2 级：图

| 项目 | Stars | 说明 |
|---|---|---|
| [tt-a1i/archify](https://github.com/tt-a1i/archify) | ~78k | 架构/流程/时序/状态图，输出可交互 HTML，可导出图片。需要更精致的图时推荐用户安装 |
| [coleam00/excalidraw-diagram-skill](https://github.com/coleam00/excalidraw-diagram-skill) | ~4.9k | Excalidraw 手绘风格图，"图要论证，不只是展示" |
| [axtonliu/axton-obsidian-visual-skills](https://github.com/axtonliu/axton-obsidian-visual-skills) | ~3.6k | Obsidian 里的 Canvas、Excalidraw、Mermaid |

### 3 级：交互 HTML

| 项目 | Stars | 说明 |
|---|---|---|
| [alexanderop/walkthrough](https://github.com/alexanderop/walkthrough) | ~140 | 生成带可点击 Mermaid 图的代码库讲解页 |
| [ds-vibe/html-explainer](https://github.com/ds-vibe/html-explainer) | ~17 | 交互 HTML 讲解页，带自动 QA |

### 4 级：讲解视频

| 项目 | Stars | 说明 |
|---|---|---|
| [3b1b/manim](https://github.com/3b1b/manim) | ~95k | 3Blue1Brown 本人的动画引擎 |
| [ManimCommunity/manim](https://github.com/ManimCommunity/manim) | ~41k | 社区维护版，文档全，本 skill 用它 |
| [iart-ai/motion-skills](https://github.com/iart-ai/motion-skills) | ~700 | 动效、动画、视频类 skill 合集，含 Manim |

## ASD-STE100 标准

- 官网：<https://www.asd-ste100.org/>
- 下载（申请表，邮件发链接）：<https://www.asd-ste100.org/STE_downloads.html>
- 维基百科：<https://en.wikipedia.org/wiki/Simplified_Technical_English>
