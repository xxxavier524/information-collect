# 来源与致谢

这个 skill 是对下列项目的综合改写，没有整段照搬。星数为 2026-10-05 查询时的数字。

| 项目 | Stars | 许可证 | 用在哪里 |
|---|---|---|---|
| [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills)（原 forrestchang/andrej-karpathy-skills） | ~217k | MIT | 第 1–4 条的主体：四原则、检验标准、"命令式→可验证目标"的改写表 |
| [obra/superpowers](https://github.com/obra/superpowers) | ~296k | MIT | 第 1 条"区分用户说的和我推断的"（brainstorming）；第 7 条"没有新鲜证据不许宣称完成"（verification-before-completion） |
| [mattpocock/skills](https://github.com/mattpocock/skills) | ~277k | MIT | 第 1 条"事实自己查、决策交给用户；一轮问完可问的问题并附推荐答案"（grilling） |
| [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) | ~110k | Apache-2.0 | 第 5、8 条：结论先行、载荷原样保留、否定词不能丢；第 2–4 条的补充：non-goals、在负责的那一层修、通过/失败/无法验证/阻塞四分法（lean-build、surgical-patch、verify-and-stop） |
| [blader/humanizer](https://github.com/blader/humanizer) | ~54k | MIT | 第 6、8 条：AI 腔分类（"不是X而是Y"、段尾金句、凑三项、加粗泛滥）、不给已知背景的读者重复解释、不编造事实 |
| [ayghri/i-have-adhd](https://github.com/ayghri/i-have-adhd) | ~54k | MIT | 第 5、6 条：第一句是可执行动作、编号步骤、只给一个下一步、岔题单独提、列表最多五项；"连续三轮没解决就停下来查假设" |
| [hardikpandya/stop-slop](https://github.com/hardikpandya/stop-slop) | ~18k | MIT | 第 8 条：删填充词、具体胜过抽象、无出处的权威 |

## Karpathy 原帖

[x.com/karpathy/status/2015883857489522876](https://x.com/karpathy/status/2015883857489522876)（2026-01）

andrej-karpathy-skills 的 README 引用了其中三段：

> "The models make wrong assumptions on your behalf and just run along with them without checking. They don't manage their confusion, don't seek clarifications, don't surface inconsistencies, don't present tradeoffs, don't push back when they should."

> "They really like to overcomplicate code and APIs, bloat abstractions, don't clean up dead code... implement a bloated construction over 1000 lines when 100 would do."

> "They still sometimes change/remove comments and code they don't sufficiently understand as side effects, even if orthogonal to the task."

以及第四条原则对应的那句：

> "LLMs are exceptionally good at looping until they meet specific goals... Don't tell it what to do, give it success criteria and watch it go."
