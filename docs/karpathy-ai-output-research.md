# Karpathy 四原则与相关 GitHub 项目调研

调研日期：2026-10-05。星数为当天 GitHub 搜索结果。

## 一、出处说明

"四个部分"不是 Karpathy 原帖里的四个小标题。原帖是 2026 年 1 月他在 X 上发的一篇长文《A few random notes from claude coding quite a bit last few weeks》，分了 Coding workflow、Tenacity、Speedups、Leverage、Fun、Atrophy、Slopacolypse 等多个小节，其中一段集中吐槽了模型写代码时的毛病。

开发者 Jiayuan Chang（forrestchang）把这段吐槽和 Leverage 一节的观点提炼成一个 65 行左右的 `CLAUDE.md`，归纳为四条原则，发布为 [andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills)（现迁到 multica-ai 组织下，约 21.7 万星）。中文圈说的"卡帕西四原则"指的就是这四条。

原帖链接：<https://x.com/karpathy/status/2015883857489522876>

## 二、四个部分

| # | 原则 | 一句话 | 针对 Karpathy 说的哪个问题 |
|---|---|---|---|
| 1 | **Think Before Coding** 先想后做 | 不假设，不藏着困惑，把取舍摆出来 | "替你做了错误的假设然后一路跑下去，不管理困惑，不澄清，不暴露矛盾，不给取舍，不在该反对时反对" |
| 2 | **Simplicity First** 简单优先 | 用最少的代码解决问题，不做投机性的东西 | "特别喜欢把代码和 API 搞复杂，抽象臃肿，不清理死代码，100 行能搞定的写成 1000 行" |
| 3 | **Surgical Changes** 外科手术式修改 | 只碰必须碰的，只收拾自己的烂摊子 | "会把自己没看懂的注释和代码当副作用改掉或删掉，哪怕跟任务无关" |
| 4 | **Goal-Driven Execution** 目标驱动 | 先定义成功标准，循环直到验证通过 | "LLM 特别擅长循环到满足具体目标为止……别告诉它做什么，给它成功标准，看它跑" |

每条的要点：

1. **先想后做**：明说假设；多种理解时列出来，不悄悄选；有更简单的方案就说；不懂就停下来问。
2. **简单优先**：不加没要的功能；一次性代码不抽象；不加没要的"可配置"；不为不可能的情况写错误处理；200 行能写成 50 行就重写。检验：资深工程师会不会嫌复杂。
3. **外科手术式修改**：不顺手改旁边的代码、注释、格式；不重构没坏的东西；沿用现有风格；无关死代码只提不删；只清理自己改动造成的孤儿。检验：diff 每一行都能追溯到需求。
4. **目标驱动**："加校验"→"先写非法输入测试再让它通过"；"修 bug"→"先写复现测试再让它通过"；"重构"→"前后测试都通过"。多步任务写"步骤 → 验证"计划。

## 三、GitHub 上的相关项目

### 直接对应

| 项目 | Stars | 说明 |
|---|---|---|
| [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills) | ~217k | 四原则本体。提供 `CLAUDE.md`、Claude Code 插件、`skills/karpathy-guidelines/SKILL.md`、Cursor 规则 |
| [richling98/karpathy-codex-skills](https://github.com/richling98/karpathy-codex-skills) | 5 | Codex 移植版 |

### 补"做事"这一层

| 项目 | Stars | 和四原则的关系 |
|---|---|---|
| [obra/superpowers](https://github.com/obra/superpowers) | ~296k | 一整套开发方法论 skill。`brainstorming`（动手前确认意图、区分事实与假设）≈ 原则 1；`verification-before-completion`（"没有新鲜验证证据不许宣称完成"）和 `test-driven-development` ≈ 原则 4 |
| [mattpocock/skills](https://github.com/mattpocock/skills) | ~277k | `grilling` / `grill-me`：按"决策树"一轮轮追问，每题附推荐答案，事实自己查、决策交给用户 ≈ 原则 1 的强化版 |
| [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) 中的 `lean-build`、`surgical-patch`、`verify-and-stop`、`investigate-first` | ~110k | 几乎是四原则的另一种写法：显式 non-goals、在负责的那一层修、验收通过立即停、先查清原因再改 |

### 补"表达"这一层

| 项目 | Stars | 侧重 |
|---|---|---|
| [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) | ~110k | 极简表达，宣称省 65% token。结论先行、删客套、代码/路径/报错原样保留、否定词绝不能丢 |
| [blader/humanizer](https://github.com/blader/humanizer) | ~54k | 基于维基百科"Signs of AI writing"，26 类 AI 腔，按强弱排序；强调改写不能增删事实 |
| [ayghri/i-have-adhd](https://github.com/ayghri/i-have-adhd) | ~54k | 为注意力有限的读者组织输出：第一行是可执行动作、编号步骤、每轮重述进度、岔题单独提、列表最多 5 项 |
| [hardikpandya/stop-slop](https://github.com/hardikpandya/stop-slop) | ~18k | 去 AI 腔清单 + 五维打分（直接性、节奏、信任、真实感、密度） |

### 集合类（找更多 skill 用）

| 项目 | Stars |
|---|---|
| [anthropics/skills](https://github.com/anthropics/skills) | ~180k |
| [VoltAgent/awesome-agent-skills](https://github.com/VoltAgent/awesome-agent-skills) | ~35k |
| [travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) | ~15k |
| [jnMetaCode/superpowers-zh](https://github.com/jnMetaCode/superpowers-zh)（superpowers 中文版） | ~8k |

## 四、怎么结合

四原则管的是**做事**：想清楚、少写、少改、可验证。它基本不管**怎么说**，而用户实际感受到的"不规范"很多出在表达上：开场白、复述、"应该可以了"、AI 腔、结论埋在最后。

所以 skill 分两层，各四条，对称：

| 层 | 规则 | 主要来源 |
|---|---|---|
| 做事 | 1 先想后做 | Karpathy + superpowers/brainstorming + mattpocock/grilling |
| 做事 | 2 简单优先 | Karpathy + caveman/lean-build（non-goals） |
| 做事 | 3 外科手术式修改 | Karpathy + caveman/surgical-patch + i-have-adhd（岔题单独提） |
| 做事 | 4 目标驱动 | Karpathy + caveman/verify-and-stop（四分法、达标即停） |
| 表达 | 5 结论先行 | caveman + i-have-adhd |
| 表达 | 6 只说有用的 | i-have-adhd + humanizer §26 |
| 表达 | 7 有证据才下结论 | superpowers/verification-before-completion |
| 表达 | 8 写人话 | humanizer + stop-slop + caveman（载荷原样），补了中文 AI 腔 |

另外加了两部分：

- **什么时候打破规则**：安全、不可逆操作、用户要求详细解释、用户没看懂、连续三轮没解决、写进 commit/文档等持久化内容。来自 caveman 和 i-have-adhd 的 "When to break the rules"。
- **交付前自检**：做事 4 问 + 表达 5 问。

几个取舍：

- **没有照搬 caveman 的"原始人语气"**。省 token 是它的目标，可读性会下降；这里只取"结论先行、删客套、载荷原样"。
- **没有照搬 superpowers 的硬性审批门**。它要求每个改动都先经用户批准设计，对小任务太重；这里用四原则原本的"琐碎任务自己判断"。
- **补了中文 AI 腔**。上面几个项目都是英文语料，"值得注意的是""赋能/闭环""不是…而是…"这类中文套路要单独列。

成品：[`skills/karpathy-output-discipline/`](../skills/karpathy-output-discipline/SKILL.md)
