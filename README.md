# Elearning · 专四口语思辨陪练

回合制英语思辨口语陪练 Skill，面向专四（TEM-4）口试备考。开源、纯文本、无外部依赖。

> 暂不支持：网页抓取、PDF 处理、图形界面。全程为纯文本回合对话。

## 1. 项目解决什么问题

专四口试要求学生就给定话题用英语即时思辨作答。多数备考者的问题是：能写但不能说、开口即中式英语、缺话题素材、练完没人给针对性的口语反馈。本项目提供一位**回合制 AI 思辨口语陪练**：

- **给出话题**：每个回合抛出一个可辩论的英文思辨题，并附一句肯定立场（Pro）、一句否定立场（Con）的纯英文参考句型，帮你在开口前就有话可说、有立场可选。
- **即时纠错**：你口头作答后，AI 逐字转写你的原话，生成 1 版专四适配的优化句，并点明本次的中式英语问题（语序 / 搭配 / 逻辑 / 母语负迁移），针对性打磨「开口」而非「写作」能力。
- **闭环训练**：通过【继续】同话题深度追问、【重新回答】同题重答，反复练习直至熟练，而不是练一道就换一道。

## 2. 主要功能

- **回合式出题**：每回合先输出英文思辨 **Topic**，再给出两句纯英文参考句型——一句肯定立场（Pro）、一句否定立场（Con）。
- **作答后三段反馈**：
  - ①【我的原句】—— 逐字转写你的口述英文，保留原始表达，不改错、不润色；
  - ②【优化句】—— 仅 1 版专四适配改写：优先修正中式英语（中文直译语序、中式搭配、逻辑断层、母语负迁移），拒绝浅层同义词替换、必须动句式与逻辑，改动核心加粗，适配口头表达、不过度书面化；
  - ③【简短点评】—— 点明本次中式英语的类别，并简述逻辑、句式、用词地道度。
- **分支闭环**：
  - 【继续】→ 不换话题，基于当前话题提出**深度追问**作为新一轮题目（同样附一正一反参考句型）；
  - 【重新回答】→ 不换题，等待你重答同一题，再次输出三段反馈，循环直至你输入【继续】。
- **可加载即用**：`SKILL.md` 驱动回合流程，`agents/openai.yaml` 供 OpenAI 兼容运行时加载为独立 Agent。

## 3. 安装方法

本项目是纯文本 Skill，**无 Python 依赖、无第三方包、无需额外安装**，只有两种「放进去」的方式：

**方式 A：放入 Agent 的 skills 目录（最常用）**

把整个 `Elearning/` 目录放到你所用 Agent 可加载的 skills 目录下（该目录位置依运行环境而定，用你所用 Agent 的 skills 目录即可），保证最终结构为：

```
<skills 目录>/
└── Elearning/
    ├── SKILL.md
    └── agents/
        └── openai.yaml
```

**方式 B：用 OpenAI 兼容运行时加载**

若使用支持 Agent 配置的 OpenAI 兼容运行时，直接用 `Elearning/agents/openai.yaml` 作为该陪练 Agent 的定义加载即可。

安装完成后，即可进入「使用方法」。

## 4. 使用方法

1. 向 Agent 发起口语思辨练习，例如：
   - 「练专四口语思辨」
   - 「陪练英语思辨表达」
   - 「帮我练口语并纠错」
2. Agent 就绪后**不会直接出题**，而是提示你回复**启用口令「开始吧」**。
3. 收到「开始吧」后，Agent 才开启回合式对话，按下方流程自动陪练：

| 阶段 | 说明 |
| --- | --- |
| 启用 | 收到口令「开始吧」后才进入训练；此前只提示用户回复「开始吧」，不出题 |
| 出题 | 输出英文 **Topic** + 一句 Pro + 一句 Con 参考句型，等待你作答 |
| 作答后 | 依次输出 ①【我的原句】②【优化句】③【简短点评】 |
| 选项 | 反馈结束后给出「【继续】」「【重新回答】」 |
| 【继续】 | 同话题深度追问，开启新一轮出题与作答 |
| 【重新回答】 | 不换题，等待你重答同一题，再次输出三段反馈，循环直至【继续】 |

4. 直接口述你的英文作答，无需其他指令；每轮作答后 AI 会给出反馈和下一步选项。

## 5. 输入输出示例

**输入（用户）：** 练专四口语思辨

**AI（未启用，仅就绪提示）：** 已就绪。请回复「开始吧」开始训练。

**输入（用户）：** 开始吧

**AI 出题：**
> **Topic:** Should college students be encouraged to take part-time jobs?
> **参考句型（Pro）:** I believe taking a part-time job during college is worthwhile, because it gives students hands-on experience and a better sense of responsibility.
> **参考句型（Con）:** However, I think a part-time job may distract students from their studies, especially when it takes up too much of their time.

**输入（用户作答）：** I think part-time job is very good for student, because it can improve our ability and also earn money. But we should not let it affect our study.

**AI 三段反馈：**
> **①【我的原句】** I think part-time job is very good for student, because it can improve our ability and also earn money. But we should not let it affect our study.
>
> **②【优化句】** I think taking a part-time job is beneficial for college students, because it can **develop practical abilities** and **help us earn some extra money**. However, we should **make sure it does not interfere with our studies**.
>
> **③【简短点评】** 中式英语问题：①"part-time job is very good for student" 属中文直译 + 冠词/单复数缺失；②"improve our ability" 搭配过泛，宜具体化为 `develop practical abilities`。逻辑有利弊两层、结构清晰；句式口语化、无需书面化；冠词、单复数和动宾搭配需打磨。

**AI 选项：**
> **【继续】**
> **【重新回答】**
