<div align="center">

# 🎓 thesis-blind-review

**学位论文盲审模拟器 · Agent Skill**

让 AI 扮演盲审专家给你的论文打分，<br>但逼它把**每一条意见都指到具体章节**。

[![License: MIT](https://img.shields.io/badge/license-MIT-informational.svg)](LICENSE)
[![Format: Agent Skill](https://img.shields.io/badge/format-Agent_Skill-8A2BE2)](SKILL.md)
[![Deps: prompt only](https://img.shields.io/badge/deps-prompt_only-success)](assets/prompt.txt)
[![Lang: 中文](https://img.shields.io/badge/lang-%E4%B8%AD%E6%96%87-critical)](README.md)

`SKILL.md` 格式 · 纯提示词 · 零依赖 · Claude Code / DSH / 任何支持 Agent Skills 的宿主

*An Agent Skill that simulates thesis blind review (盲审) — rubric-anchored scoring with paragraph-locatable comments.*

</div>

---

## 一句话

不是"让 AI 夸你论文"，是**逼它说清楚问题在哪一行**。

## ✨ 输出什么

给一篇硕士学位论文（全文 / PDF / 截图），输出一份像盲审专家写的评审：

| | 输出项 | 说明 |
|---|---|---|
| 📊 | **四维度评分** | 学术性 / 规范性 / 创新性 / 完整性，各占 25%，百分制 |
| 🧮 | **加权总分** | `Σ(维度得分 × 25%)`，三档锚点校准，同篇复评漂移 ≤ 5 分 |
| 🚫 | **否决项判定** | 抄袭 / 数据造假 / 逻辑硬伤 / 方法错误 / 格式混乱，触发即 C 档 |
| 📍 | **可定位意见** | 每条指到章节号 / 页码 / 图表号，指不到 = 无效意见 |
| 🛠️ | **修改清单** | 「必须修改」「建议修改」分栏，按优先级排序 |
| 🎓 | **答辩建议** | 同意答辩 / 修改后答辩 / 不同意答辩——对上学校盲审表的真实口径 |

## 🚀 四种用法

**1 · 装成 skill**

```bash
# Claude Code
git clone https://github.com/coincidence2020/thesis-blind-review.git ~/.claude/skills/thesis-blind-review

# DSH 及其他宿主：把本目录放进 profile 的 skills 目录即可
```

然后对它说：

```text
帮我盲审这篇论文（附全文 / PDF / 截图）
```

**2 · 让你的 AI 自己装**

已经在用 Agent 宿主（Claude Code / DSH 等）的，把这段话直接发给它，克隆、安装、确认一气呵成：

```text
帮我安装这个skill：https://github.com/coincidence2020/thesis-blind-review.git
克隆到你当前环境的 skills 目录（比如 Claude Code 的 ~/.claude/skills/，
其他宿主按你自己的约定来）。装好后读一遍 SKILL.md 确认能被加载，
然后告诉我装好了。
```

**3 · 只要提示词**

[`assets/prompt.txt`](assets/prompt.txt) 是自包含的完整提示词（约 2.2k 字），粘进任意模型——GPT / Claude / Gemini / DeepSeek / GLM / Kimi——就能用，不需要装 skill。

**4 · 只要细则**

[`references/rubric.md`](references/rubric.md)（12 关注点 + 评分锚点）、[`references/veto-items.md`](references/veto-items.md)（否决项判定）可单独拿去改造成你自己的版本。

## 📦 目录结构

```text
thesis-blind-review/
├── SKILL.md                     主文件：主流程、五条纪律、常见坑（99 行）
├── assets/
│   ├── prompt.txt               完整提示词，单文件可粘贴
│   └── output-template.md       输出骨架（评审范围 → 意见 → 清单 → 等级）
├── references/
│   ├── rubric.md                12 关注点细则 + 三档评分锚点
│   ├── veto-items.md            否决性指标判定细则
│   ├── venues.md                分学科顶会顶刊参考清单
│   └── confidentiality.md       送审前保密与脱敏检查
└── examples/
    └── sample-review.md         虚构论文的完整评审示例（看输出粒度）
```

<details>
<summary><b>📁 每个文件什么时候被读</b>（按需加载，不是一次全读）</summary>

| 文件 | 何时读 |
|---|---|
| `references/rubric.md` | 每次评审，12 条细则与三档锚点 |
| `references/veto-items.md` | 判定否决项时 |
| `references/venues.md` | 判断"文献综述够不够"时，按学科查顶会顶刊 |
| `references/confidentiality.md` | 评审前提醒用户保密与脱敏 |
| `assets/prompt.txt` | 用户想要可粘贴到任意模型的提示词 |
| `assets/output-template.md` | 生成输出时 |
| `examples/sample-review.md` | 不确定输出粒度时，看示例 |

</details>

<details>
<summary><b>🧠 五条评审纪律</b>（SKILL.md 里违反任一条输出即作废）</summary>

| 纪律 | 具体要求 |
|---|---|
| **不臆造** | 读不到的内容写"无法判定"；不许推测缺失章节的内容 |
| **不臆断文献** | 不能联网核实文献是否存在。只标可疑特征，列「待核实清单」，**不得断言某篇文献不存在** |
| **不空谈** | 无位置的意见无效。写不出章节号，说明这条意见本身没依据 |
| **不越权** | 结论是"模拟评审参考"，不承诺"能过/不能过"，只给风险区间 |
| **不改稿** | 用户没要求就不重写论文，只指出问题与改法 |

</details>

## ⚖️ 与"帮我打个分"的区别

| 常见做法 | 结果 | 本 skill 的做法 |
|---|---|---|
| "请评审这篇论文" | 2000 字套话，全是"创新性有待加强" | 12 个关注点逐条，每条必须引用章节号 / 图表号 |
| 不锁评分标准 | 同一篇评两次差 10 分 | 三档评分锚点 + 加权公式，漂移压到 5 分内 |
| 让它"检查有没有假文献" | 模型无法联网，只能编 | 只标可疑特征，列待核实清单，明令"不得断言不存在" |
| 只发摘要 | 模型顺着摘要补全整篇 | 输出开头强制写「评审范围」，缺失部分标"无法判定" |
| 只有 A/B/C | 不知道能不能答辩 | 附答辩建议（同意 / 修改后 / 不同意），对上学校盲审表 |

## 🧭 设计取舍

<details>
<summary><b>为什么四维度等权、不设 D 档、不联网核实文献？</b>（点开看四条）</summary>

<br>

**1 · 四维度等权，不做加权。**
各校权重不同（有的把创新性提到 40%）。默认等权、写明公式，用户要改就改一行，比内置一堆学校规则表更耐用。

**2 · 否决项 → C 档，不设 D 档。**
多数学校盲审只有三档口径。学术不端单独降级会与"抄袭 → C"打架，改为在 C 的结论句里强制写"不建议答辩 / 延期答辩"。

**3 · 不联网核实文献，也不假装能。**
这是本 skill 最反直觉的一条：让模型"检查幻觉引用"会**制造**幻觉。正确做法是标可疑特征 + 列清单 + 把判断权交回人。

**4 · 只给风险区间，不给"能不能过"。**
模型不认识你的评审专家。它能做的是指出"这三个创新点里有两个没有消融实验支撑"，不是预测 84 分。

</details>

## ⚠️ 局限

- **学科适配有限** —— 规则偏计算机 / 工程类；人文社科（尤其质性研究）的工作量口径差异很大，需人工改写 `references/rubric.md`
- **读图能力决定上限** —— 公式推导、图表细节、排版问题，纯文本输入会漏
- **不能替代真盲审** —— 自查工具，用于送审前发现"自己看不出来"的结构性问题
- **不判断学术不端** —— 抄袭靠查重系统，造假靠复现，不是 LLM 能定的

## 🔒 合规与免责

> - 本工具输出**仅供参考**，不构成任何正式评审意见，**不得作为申诉、答辩、学位授予的依据**
> - **上传论文前请自行确认**：是否已送审、是否涉密、学校是否允许使用第三方 AI 服务（详见 [`references/confidentiality.md`](references/confidentiality.md)）
> - 请勿用于规避学术审查、代写、伪造评审意见

## 📄 License

[MIT](LICENSE) © 2026 thesis-blind-review contributors

<div align="center">

---

**如果它帮你提前发现了三个你自己没看见的问题，star 一下** ⭐

</div>
