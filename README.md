# thesis-blind-review

> 中文学位论文盲审模拟器 —— 一个 Agent Skill（SKILL.md 格式，Claude Code / DSH / 其他支持 Agent Skills 的宿主通用）

给一篇硕士学位论文，按国内盲审口径输出：四维度分数、加权总分、A/B/C 等级、**可定位到章节的意见**、修改清单、答辩建议。

不是"让 AI 夸你论文"，是**逼它说清楚问题在哪一行**。

## 30 秒上手

把 `SKILL.md` 所在目录放进宿主的 skills 目录，然后说：

```
帮我盲审这篇论文（附全文/PDF/截图）
```

或者直接要一份提示词，粘到任意模型（GPT / Claude / Gemini / DeepSeek / GLM）里用：

```
读取 assets/prompt.txt，我要用它评审我的论文
```

## 为什么不用"给我打个分"这种提示词

| 常见做法 | 结果 | 这个 skill 的做法 |
|---|---|---|
| "请评审这篇论文" | 输出 2000 字套话，全是"创新性有待加强" | 12 个关注点逐条，每条必须引用章节号/图表号，否则判无效 |
| 不锁评分标准 | 同一篇评两次差 10 分 | 三档评分锚点 + 加权公式，把漂移压到 5 分内 |
| 让它"检查有没有假文献" | 模型无法联网，只能编 | 只标可疑特征，列待核实清单，明令"不得断言不存在" |
| 只发摘要 | 模型顺着摘要补全整篇 | 输出开头强制写「评审范围」，缺失部分标"无法判定" |
| 只有 A/B/C | 不知道能不能答辩 | 附答辩建议（同意 / 修改后 / 不同意），对上学校盲审表 |

## 包含什么

```
SKILL.md                      主文件：怎么跑、纪律、常见坑
assets/
  prompt.txt                  完整提示词，单文件可复制（约 1.5k 字）
  output-template.md          输出骨架（评审范围→意见→清单→等级）
references/
  rubric.md                   四维度 12 关注点 + 三档评分锚点
  veto-items.md               否决性指标判定细则
  venues.md                   分学科顶会顶刊参考清单
  confidentiality.md          送审前保密与脱敏检查
examples/
  sample-review.md            虚构论文的完整评审示例（看粒度）
```

## 设计取舍

**1. 四维度等权，不做加权。**
各校权重不同（有的把创新性提到 40%）。默认等权、并写明公式，用户要改就改一行，比内置一堆学校规则表更耐用。

**2. 否决项→C 档，不设 D 档。**
多数学校盲审只有三档口径。学术不端单独降级会与"抄袭→C"打架，改为在 C 的结论句里强制写"不建议答辩/延期答辩"。

**3. 不联网核实文献，也不假装能。**
这是本 skill 最反直觉的一条：让模型"检查幻觉引用"会**制造**幻觉。正确做法是标可疑特征 + 列清单 + 把判断权交回人。

**4. 只给风险区间，不给"能不能过"。**
模型不认识你的评审专家。它能做的是指出"这三个创新点里有两个没有消融实验支撑"，不是预测 84 分。

## 局限

- **学科适配有限**：规则偏计算机/工程类；人文社科（尤其质性研究）的"工作量"口径差异很大，需要人工改写 `references/rubric.md`。
- **读图能力决定上限**：公式推导、图表细节、排版问题，纯文本输入会漏。
- **不能替代真盲审**：它是自查工具，用于在送审前发现"自己看不出来"的结构性问题。
- **不判断学术不端**：抄袭要靠查重系统，数据造假要靠复现，不是 LLM 能定的。

## 合规与免责

- 本工具输出**仅供参考**，不构成任何正式评审意见，不得作为申诉、答辩、学位授予的依据。
- **上传论文前请自行确认**：是否已送审、是否涉密、学校是否允许使用第三方 AI 服务。详见 `references/confidentiality.md`。
- 请勿用于规避学术审查、代写、伪造评审意见。

## 来源与致谢

初始提示词来自小红书用户 **cc（沉迷仔仔版）** 在笔记《Ai论文盲审结果，claude和书生是神》置顶评论中分享的版本（[原帖](https://www.xiaohongshu.com/discovery/item/69dc73340000000023025bc2)）。

本仓库在其基础上做了系统性重写：修掉等级表与输出格式的矛盾、补总分算法与评分锚点、加入文献真实性处理与信息缺失处理、加入保密检查、补充分学科venue清单。详见 [ATTRIBUTION.md](ATTRIBUTION.md)。

## License

MIT —— 见 [LICENSE](LICENSE)。

## 发布

```bash
git init -b main
git add .
git commit -m "feat: thesis blind review skill"
# 在 GitHub 网页端新建空仓库后：
git remote add origin https://github.com/coincidence2020/thesis-blind-review.git
git push -u origin main
```

或先装 GitHub CLI 再一条命令建库：

```bash
winget install GitHub.cli
gh auth login
gh repo create thesis-blind-review --public --source=. --push
```
