# information-collect

## karpathy-output-ladder

按 Karpathy 2026-10-02 推文的四种格式，把 AI 的输出做成人能看懂、能核对的形式：

1. 受控文字（约 80% 的 ASD-STE100；中文用受控中文）
2. 图（Mermaid / SVG）
3. 交互 HTML
4. 3Blue1Brown 风格的讲解视频（Manim + 配音）

- Skill：[`skills/karpathy-output-ladder/SKILL.md`](skills/karpathy-output-ladder/SKILL.md)
- 调研笔记：[`docs/karpathy-output-ladder-research.md`](docs/karpathy-output-ladder-research.md)

### 安装（Claude Code）

所有项目可用：

```bash
mkdir -p ~/.claude/skills
cp -r skills/karpathy-output-ladder ~/.claude/skills/
```

只在某个项目用：

```bash
mkdir -p .claude/skills
cp -r skills/karpathy-output-ladder .claude/skills/
```

### 用法示例

- "用 80% STE 解释一下这个 PR 改了什么"
- "把刚才的解释画成图"
- "做一个 HTML 页面，让我能拖动参数看 attention 的变化"
- "做一个 3b1b 风格的视频讲解 TCP 三次握手"

1 级只需要 Python 3。2 级不需要额外依赖。3 级需要浏览器。4 级需要 ffmpeg、Manim（依赖 Cairo 和 Pango），以及一种配音工具（ElevenLabs API key、Piper，或 macOS 自带的 `say`）。
