# 4 级：讲解视频

Karpathy："我最看好的输出格式是完全定制的讲解视频，任何主题都可以生成。"他的示例：让模型做一个"3b1b 风格的讲解视频"，用他的 ElevenLabs API key 配音；或者让模型找能用本地算力的免费替代方案。

## 目录结构

```
explainers/<主题>/
├── narration.md          # 按拍编号的旁白
├── scene.py              # Manim 场景，类名 Explainer
├── audio/
│   ├── beat_01.wav …     # 每拍一个音频
│   ├── durations.json    # 每拍时长，场景按它等待
│   └── narration.wav     # 拼好的整段旁白
└── <主题>.mp4            # 成片
```

## 第 1 步：写旁白

`narration.md` 是编号列表，一项一拍。每拍 1～3 句受控文字，配一个动画步骤。每拍约 4～8 秒：中文 20～40 字，英文 10～20 词。整片 8～20 拍。

```markdown
1. 梯度下降用来找函数的最低点。
2. 这是函数 x 的平方。最低点在 x 等于 0。
3. 我们从 x 等于 4 出发。这里的斜率是 8，方向朝上。
4. 所以我们往反方向走一小步。步长等于学习率乘以斜率。
```

旁白是听的，不是看的：
- 符号写成读音。"x²"写"x 的平方"，"≥"写"大于等于"。
- 少用缩写。第一次出现的英文缩写念全称。
- 遵守严格度，写完跑 linter：`python3 <skill目录>/scripts/ste-lint-zh.py narration.md`。

## 第 2 步：配音

按顺序选：

| 条件 | 用什么 |
|---|---|
| 环境里有 `ELEVENLABS_API_KEY` | ElevenLabs，质量最好 |
| 已装 Piper | Piper，本地运行，有中文音色 |
| 已装 Kokoro 或用户给了 `TTS_CMD` | 用它 |
| macOS | 系统自带 `say`，中文音色如 `Tingting` |
| 都没有 | 不配音，只交付分镜（见最后一节），并说明装什么 |

**不经用户同意，不全局安装软件，不下载语音模型。**

每拍生成一个文件 `audio/beat_NN.wav`。例子：

```bash
# ElevenLabs（voice_id 先用 GET https://api.elevenlabs.io/v1/voices 查）
curl -sS -X POST "https://api.elevenlabs.io/v1/text-to-speech/$VOICE_ID" \
  -H "xi-api-key: $ELEVENLABS_API_KEY" -H "Content-Type: application/json" \
  -d '{"text": "梯度下降用来找函数的最低点。", "model_id": "eleven_multilingual_v2"}' \
  -o audio/beat_01.mp3
ffmpeg -loglevel error -y -i audio/beat_01.mp3 -ar 44100 -ac 1 audio/beat_01.wav

# Piper（模型文件需事先下载，例如 zh_CN-huayan-medium.onnx）
echo "梯度下降用来找函数的最低点。" | piper --model zh_CN-huayan-medium.onnx --output_file audio/beat_01.wav

# macOS
say -v Tingting -o audio/beat_01.aiff "梯度下降用来找函数的最低点。"
ffmpeg -loglevel error -y -i audio/beat_01.aiff -ar 44100 -ac 1 audio/beat_01.wav
```

所有拍统一成同样的采样率和声道（上面用 44100 Hz 单声道），后面才能直接拼接。

然后算每拍时长，写进 `durations.json`：

```bash
python3 - <<'EOF'
import json, subprocess, pathlib
d = {}
for f in sorted(pathlib.Path("audio").glob("beat_*.wav")):
    n = int(f.stem.split("_")[1])
    out = subprocess.run(["ffprobe", "-v", "error", "-show_entries", "format=duration",
                          "-of", "csv=p=0", str(f)], capture_output=True, text=True, check=True)
    d[str(n)] = round(float(out.stdout), 3)
pathlib.Path("audio/durations.json").write_text(json.dumps(d, indent=1))
print(d)
EOF
```

## 第 3 步：写 Manim 场景

用 Manim Community 版。它需要系统里有 Cairo 和 Pango（Ubuntu：`sudo apt install libcairo2-dev libpango1.0-dev pkg-config`，其他系统看 Manim 官方安装文档），然后 `uv tool install manim` 或 `pip install manim`。这些都要先问用户。

一个类 `Explainer`，一拍一组 `self.play(...)`，用 `# 第 N 拍` 注释对齐旁白。每拍结束时按 `durations.json` 补足等待时间，让画面和声音对齐：

```python
from manim import *
import json, pathlib

HERE = pathlib.Path(__file__).parent
DUR_FILE = HERE / "audio" / "durations.json"
DUR = json.loads(DUR_FILE.read_text()) if DUR_FILE.exists() else {}
FONT = "Noto Sans CJK SC"  # 中文需要能显示中文的字体；macOS 可用 "PingFang SC"

def hold(scene, beat, used):
    """等到第 beat 拍的旁白结束。used 是这一拍已经播放的动画秒数。"""
    scene.wait(max(0.3, DUR.get(str(beat), used + 1.0) - used))

class Explainer(Scene):
    def construct(self):
        # 第 1 拍
        title = Text("梯度下降", font=FONT, font_size=56)
        self.play(Write(title), run_time=1.5)
        hold(self, 1, 1.5)

        # 第 2 拍
        self.play(FadeOut(title), run_time=0.5)
        axes = Axes(x_range=[-5, 5, 1], y_range=[0, 25, 5], x_length=8, y_length=4.5)
        curve = axes.plot(lambda x: x ** 2, color=BLUE)
        self.play(Create(axes), Create(curve), run_time=2)
        hold(self, 2, 2.5)

        # 第 3 拍：用 ValueTracker 驱动一个随参数移动的点
        x = ValueTracker(4)
        dot = always_redraw(lambda: Dot(axes.c2p(x.get_value(), x.get_value() ** 2), color=YELLOW))
        self.play(FadeIn(dot), run_time=0.5)
        hold(self, 3, 0.5)

        # 第 4 拍：真的按梯度下降更新 x
        lr = 0.1
        for _ in range(5):
            self.play(x.animate.set_value(x.get_value() - lr * 2 * x.get_value()), run_time=0.4)
        hold(self, 4, 2.0)
```

3b1b 风格要点：
- 深色背景（Manim 默认），颜色少且含义固定（例如蓝色 = 函数，黄色 = 正在变化的量）。
- 一屏一个概念。下一个概念出现前，`FadeOut` 上一个。
- 用运动展示机制：`Transform` 把一种形式变成下一种；`ValueTracker` 驱动变化的参数；`always_redraw` 画依赖参数的东西。
- 公式用 `MathTex` 需要装 LaTeX；没装就用 `Text`。
- 动画里展示的计算要真的算，不要摆一条预设轨迹。

渲染：

```bash
manim -qh scene.py Explainer
# 输出在 media/videos/scene/1080p60/Explainer.mp4
```

先用 `-ql`（低清）快速预览，确认没问题再用 `-qh`。

## 第 4 步：合成

```bash
# 按顺序拼接每拍音频
for f in audio/beat_*.wav; do echo "file '$(basename "$f")'"; done > audio/list.txt
ffmpeg -loglevel error -y -f concat -safe 0 -i audio/list.txt -c copy audio/narration.wav

# 合成视频和旁白
ffmpeg -loglevel error -y -i media/videos/scene/1080p60/Explainer.mp4 -i audio/narration.wav \
  -c:v copy -c:a aac -b:a 192k explainers/<主题>/<主题>.mp4
```

合成前比较两个时长：

```bash
ffprobe -v error -show_entries format=duration -of csv=p=0 media/videos/scene/1080p60/Explainer.mp4
ffprobe -v error -show_entries format=duration -of csv=p=0 audio/narration.wav
```

相差超过 0.5 秒，回去调场景里的 `hold`。不要用 `-shortest` 掩盖差异，它会直接截掉多出的旁白或画面。

## 第 5 步：检查

抽几帧看：

```bash
for t in 1 10 20; do ffmpeg -loglevel error -y -ss $t -i <主题>.mp4 -frames:v 1 frame_$t.png; done
```

看文字有没有重叠、出画，有没有空白帧，中文有没有显示成方块（方块说明字体不对）。

## 交付

回复里写明用了哪个渲染器、哪个配音。用的是退而求其次的方案时，用一行说明换成什么会更好（Manim、ElevenLabs、Piper）以及怎么装。

## 做不出视频时：交付分镜

缺 Manim 或缺配音时，**不要说视频做好了**。交付分镜表，任何视频工具都能照着做：

| 拍 | 时长 | 画面 | 旁白 | 屏幕文字 |
|---|---|---|---|---|
| 1 | 4 秒 | 标题淡入 | 梯度下降用来找函数的最低点。 | 梯度下降 |
| 2 | 6 秒 | 画坐标轴和 x² 曲线 | 这是函数 x 的平方。最低点在 x 等于 0。 | f(x) = x² |
| … | | | | |

并说明缺什么、装了之后可以直接渲染。
