---
layout: post
title: "给真正工程师用的 Skills：Matt Pocock 仓库 README 中文译本"
date: 2026-08-25 16:53:30 +0800
---

本文是 [mattpocock/skills](https://github.com/mattpocock/skills) 仓库 README 的中文译本。原文标题为 *Skills For Real Engineers*。命令、技能名、文件名和仓库路径保持原文，便于对照安装和使用。

原文地址：[https://github.com/mattpocock/skills](https://github.com/mattpocock/skills)

这是作者日常用来做真实工程的 agent skills，不是 vibe coding。

开发真正的应用很难。GSD、BMAD、Spec-Kit 这类方法试图通过接管流程来帮忙。但它们同时拿走了你的控制权，流程里出了 bug 也很难处理。

这些 skills 被设计成小块、容易改、可以组合。它们能配合任意模型，来自数十年的工程经验。随便改，变成你自己的，然后享受这个过程。

如果想跟进这些 skills 的更新，以及作者后续新写的 skills，可以加入作者的 newsletter（原文写大约有 6 万名开发者）：

[订阅 Newsletter](https://www.aihero.dev/s/skills-newsletter)

skills.sh 徽章见：[https://skills.sh/mattpocock/skills](https://skills.sh/mattpocock/skills)

## 安装（30 秒上手）

两条路，两种理念。**[Claude Code plugin](https://code.claude.com/docs/en/plugins)** 把整套 skills 装成受管理、只读的包，作者发版时会自动更新，所以你是订阅，不是 fork。**[skills.sh](https://skills.sh/mattpocock/skills)** 把可编辑的 skill 文件复制进你的项目，方便你自己改。二选一：两个都装的话，每个 skill 都会出现两份。

### 1. 拿到 skills

**Claude Code**

```bash
claude plugins install mattpocock-skills
```

或者在会话里执行：

```text
/plugin install mattpocock-skills
```

它已经在 Claude Code 的官方 marketplace 里，不用先额外添加，更新会自动到来。

**Codex 和其他 agent**

```bash
npx skills@latest add mattpocock/skills
```

选择你要的 skills，以及要装到哪些 coding agent 上。**安装器会让你挑选要带走的 skills，请确保其中包含 `setup-matt-pocock-skills`。**

原生 Codex plugin 已在路线图中，见 [`.agents/adr/0002-ship-as-a-claude-code-plugin.md`](https://github.com/mattpocock/skills/blob/main/.agents/adr/0002-ship-as-a-claude-code-plugin.md)。

**给喜欢动手改的人**

在任意 agent 上用同一个安装器，包括 Claude Code：

```bash
npx skills@latest add mattpocock/skills
```

它会把 skills 写成你仓库里的普通文件，归你所有，可以随便改。不会在背后偷偷更新；想拉作者最新改动时，再执行 `npx skills update`。

### 2. 运行 `/setup-matt-pocock-skills`

在你的 agent 里，每个仓库跑一次。它会：

- 问你要用哪种 issue tracker（GitHub、Linear，或本地文件）
- 问你 triage 工单时会打哪些标签（`/triage` 会用到标签）
- 问你希望把生成的文档存在哪里

### 3. 搞定，可以开始了。

## 为什么会有这些 Skills

作者写这些 skills，是为了修复自己在 Claude Code、Codex 和其他 coding agent 上常见的失败模式。

### 问题 1：Agent 没按我想的做

> "No-one knows exactly what they want"
>
> David Thomas & Andrew Hunt，《[The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)》
>
> 译文：没有人真正精确知道自己想要什么。

**问题**。软件开发里最常见的失败，是认知没对齐。你以为开发已经懂你要什么。结果一看成品，才发现对方完全没理解。

AI 时代也是这样。你和 agent 之间有沟通鸿沟。修法是一次 **grilling session（拷问式访谈）**：让 agent 就你要做的东西，问你足够细的问题。

**修法**是用：

- [`/grill-me`](https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md)：非代码场景
- [`/grill-with-docs`](https://github.com/mattpocock/skills/blob/main/skills/engineering/grill-with-docs/SKILL.md)：和 [`/grill-me`](https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md) 一样，但多了一些能力（见下文）

这是作者最受欢迎的 skills。它们帮你在动手前先和 agent 对齐，并认真想清楚这次改动。每次要改东西时都该用。

### 问题 2：Agent 话太多

> With a ubiquitous language, conversations among developers and expressions of the code are all derived from the same domain model.
>
> Eric Evans，《[Domain-Driven-Design](https://www.amazon.co.uk/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)》
>
> 译文：有了通用语言，开发者之间的对话，以及代码的表达，都来自同一套领域模型。

**问题**：项目刚开始时，开发者和他们为之构建软件的人（领域专家）通常说的是不同语言。

作者和自己的 agent 也感到同样的张力。Agent 通常被直接丢进项目，边做边猜行话，于是能用 1 个词说清的事，它用了 20 个词。

**修法**是一套共享语言。它是一份文档，帮 agent 解码项目里的行话。

**例子**：作者 `course-video-manager` 仓库里的一份 [`CONTEXT.md`](https://github.com/mattpocock/course-video-manager/blob/076a5a7a182db0fe1e62971dd7a68bcadf010f1c/CONTEXT.md)。下面哪种更好读？

- **之前**："There's a problem when a lesson inside a section of a course is made 'real' (i.e. given a spot in the file system)"
- **之后**："There's a problem with the materialization cascade"

这种简洁会在一次次会话里持续兑现。

这套能力已经做进 [`/grill-with-docs`](https://github.com/mattpocock/skills/blob/main/skills/engineering/grill-with-docs/SKILL.md)。它还是一次拷问式访谈，但会帮你和 AI 建立共享语言，并把难讲清的决策写进 ADR。

这件事有多强，很难用文字讲透。它可能是这个仓库里最酷的一项技术。去试，自己看。

> **提示：** 共享语言除了减少啰嗦，还有很多好处：
>
> - **变量、函数和文件命名一致**，都用这套共享语言
> - 因此，agent **更容易在代码库里导航**
> - agent 还能 **少花一些思考 token**，因为它拿到了一套更简洁的语言

### 问题 3：代码根本不能用

> "Always take small, deliberate steps. The rate of feedback is your speed limit. Never take on a task that’s too big."
>
> David Thomas & Andrew Hunt，《[The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)》
>
> 译文：永远走小而刻意的步骤。反馈速度就是你的速度上限。永远不要接过大到失控的任务。

**问题**：假设你和 agent 已经对齐了要做什么。如果 agent *仍然* 产出垃圾，怎么办？

该看反馈循环了。如果 agent 写出来的代码实际怎么跑，它收不到反馈，就会在盲飞。

**修法**：你需要常规那一套反馈循环：静态类型、浏览器访问，以及自动化测试。

对自动化测试来说，红-绿-重构循环很关键。agent 先写一个失败的测试，再把测试修绿。这能给 agent 稳定的反馈，从而写出好得多的代码。

作者做了一个可以塞进任意项目的 **[`/tdd`](https://github.com/mattpocock/skills/blob/main/skills/engineering/tdd/SKILL.md) skill**。它鼓励红-绿-重构，并给 agent 充分说明什么是好测试、什么是坏测试。

调试方面，作者还做了 **[`/diagnosing-bugs`](https://github.com/mattpocock/skills/blob/main/skills/engineering/diagnosing-bugs/SKILL.md)** skill，把最佳调试实践包进一个有纪律的循环，按阶段把门。

### 问题 4：我们造出了一团泥球

> "Invest in the design of the system *every day*."
>
> Kent Beck，《[Extreme Programming Explained](https://www.amazon.co.uk/Extreme-Programming-Explained-Embrace-Change/dp/0321278658)》
>
> 译文：每天都要投资系统设计。

> "The best modules are deep. They allow a lot of functionality to be accessed through a simple interface."
>
> John Ousterhout，《[A Philosophy Of Software Design](https://www.amazon.co.uk/Philosophy-Software-Design-2nd/dp/173210221X)》
>
> 译文：最好的模块是深的。它们让大量功能能通过简单接口访问。

**问题**：大多数用 agent 做出来的应用又复杂又难改。agent 能大幅加快写代码，也就同时加快软件熵增。代码库会以前所未有的速度变复杂。

**修法**是一种对 AI 驱动开发来说相当激进的做法：真正关心代码设计。

这已经嵌进这些 skills 的每一层：

- [`/to-spec`](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-spec/SKILL.md) 在写 spec 之前，会先问你这次会碰到哪些模块

更关键的是，[`/improve-codebase-architecture`](https://github.com/mattpocock/skills/blob/main/skills/engineering/improve-codebase-architecture/SKILL.md) 会扫描代码库，找可以加深模块的机会，再把候选交给你。作者建议每隔几天在代码库上跑一次。它是勘察，不是救援：在真正老的代码库上，它能找到真实候选，但不会替你把泥球拆开。

### 小结

软件工程基本功比以往任何时候都更重要。这些 skills 是作者把这些基本功浓缩成可重复实践的一次尽力尝试，帮你交出职业生涯里最好的应用。享受这个过程。

## 参考

这些 skills 按一个轴切开：谁能调用它们。**User-invoked（用户调用）** skills 只有你亲手输入时才能走到（例如 `/grill-me`），职责是编排。**Model-invoked（模型调用）** skills 既可以由你调用，也可以在任务合适时由 agent 自动拿来用，它们承载可复用的纪律。一个 user-invoked skill 可以调用 model-invoked skills，但绝不能再调用另一个 user-invoked skill。

### Engineering

作者每天写代码时用的 skills。

**User-invoked**

- **[ask-matt](https://github.com/mattpocock/skills/blob/main/skills/engineering/ask-matt/SKILL.md)**：问当前局面该用哪个 skill 或流程。它是本仓库 user-invoked skills 的路由器。
- **[grill-with-docs](https://github.com/mattpocock/skills/blob/main/skills/engineering/grill-with-docs/SKILL.md)**：拷问式访谈，同时构建项目的领域模型，打磨术语，并就地更新 `CONTEXT.md` 和 ADR。
- **[triage](https://github.com/mattpocock/skills/blob/main/skills/engineering/triage/SKILL.md)**：按 triage 角色的状态机推进 issue。
- **[improve-codebase-architecture](https://github.com/mattpocock/skills/blob/main/skills/engineering/improve-codebase-architecture/SKILL.md)**：扫描代码库，寻找加深模块的机会，用可视化 HTML 报告呈现，再对你选中的那一项做拷问。
- **[setup-matt-pocock-skills](https://github.com/mattpocock/skills/blob/main/skills/engineering/setup-matt-pocock-skills/SKILL.md)**：为本仓库的 engineering skills 做配置（issue tracker、triage 标签、领域文档布局）。在使用其他 engineering skills 之前，每个仓库跑一次。
- **[to-spec](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-spec/SKILL.md)**：把当前对话变成一份 spec，并发布到 issue tracker。不做访谈，只综合你们已经讨论过的内容。
- **[to-tickets](https://github.com/mattpocock/skills/blob/main/skills/engineering/to-tickets/SKILL.md)**：把任意计划、spec 或对话拆成一组 tracer-bullet tickets，每张票声明自己的阻塞边。可以写成本地文件里的文本，也可以在真正的 tracker 上写成原生阻塞链接。
- **[implement](https://github.com/mattpocock/skills/blob/main/skills/engineering/implement/SKILL.md)**：按 spec 或一组 tickets 描述的工作来实现，在事先约定的缝上驱动 `/tdd`，提交前用 `/code-review` 收尾。
- **[wayfinder](https://github.com/mattpocock/skills/blob/main/skills/engineering/wayfinder/SKILL.md)**：规划一块大到单次 agent 会话装不下的工作，在 issue tracker 上做成决策 tickets 的共享地图，一次解决一张，直到通往目的地的路变清楚。

**Model-invoked**

- **[prototype](https://github.com/mattpocock/skills/blob/main/skills/engineering/prototype/SKILL.md)**：做一个用完就扔的原型，用来回答设计问题。状态/逻辑问题用一份可分享的 HTML 文件；UI 则做成几种差异很大的变体，从一个路由里切换。
- **[diagnosing-bugs](https://github.com/mattpocock/skills/blob/main/skills/engineering/diagnosing-bugs/SKILL.md)**：针对难 bug 和性能回退的有纪律诊断循环：先做一个会因这个 bug 变红的反馈循环 → 最小化 → 假设 → 埋点 → 修复 → 回归测试。
- **[research](https://github.com/mattpocock/skills/blob/main/skills/engineering/research/SKILL.md)**：针对高信任的一手来源调查一个问题，把发现写成带引用的 Markdown 文件放进仓库，作为后台 agent 运行。
- **[tdd](https://github.com/mattpocock/skills/blob/main/skills/engineering/tdd/SKILL.md)**：带红-绿-重构循环的测试驱动开发。一次做一个垂直切片来做功能或修 bug。
- **[domain-modeling](https://github.com/mattpocock/skills/blob/main/skills/engineering/domain-modeling/SKILL.md)**：主动构建并打磨项目的领域模型：对照术语表挑战用词，用边界场景做压力测试，并就地更新 `CONTEXT.md` 和 ADR。
- **[codebase-design](https://github.com/mattpocock/skills/blob/main/skills/engineering/codebase-design/SKILL.md)**：设计深模块的共享纪律和词汇：大量行为藏在小接口后面，放在干净的缝上，并通过该接口可测。
- **[code-review](https://github.com/mattpocock/skills/blob/main/skills/engineering/code-review/SKILL.md)**：从某个固定点以来的 diff 做双轴审查：**Standards**（是否遵循仓库编码标准，外加 Fowler smell 基线？）和 **Spec**（是否忠实实现了发起这次工作的 issue/spec？）。用并行子 agent 跑，避免互相污染。
- **[resolving-merge-conflicts](https://github.com/mattpocock/skills/blob/main/skills/engineering/resolving-merge-conflicts/SKILL.md)**：处理进行中的 git merge 或 rebase 冲突，一块一块来，按各方一手来源追溯意图后解决，然后完成这次操作（绝不 `--abort`）。
- **[wizard](https://github.com/mattpocock/skills/blob/main/skills/engineering/wizard/SKILL.md)**：生成一个交互式 bash wizard，带人走只有人能做的步骤：开通基础设施、配置凭证或 CI secrets、操作不熟悉的第三方控制台，或跑一次性迁移/切换。

### Productivity

通用工作流工具，不局限于写代码。

**User-invoked**

- **[grill-me](https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md)**：就计划或设计被不停追问，直到设计树的每个分支都落地。
- **[handoff](https://github.com/mattpocock/skills/blob/main/skills/productivity/handoff/SKILL.md)**：把当前对话压缩成一份交接文档，让另一个 agent 能接着干。
- **[teach](https://github.com/mattpocock/skills/blob/main/skills/productivity/teach/SKILL.md)**：用多会话教用户一项新技能或概念，把当前目录当成有状态的教学工作区。
- **[to-questionnaire](https://github.com/mattpocock/skills/blob/main/skills/productivity/to-questionnaire/SKILL.md)**：把你一个人答不了的决策，变成一份给那个能答的人的 Markdown 问卷，可以异步填，也可以开会一起填。它拷问的是这次发送（给谁、需要对方回什么），不是问题主题本身。
- **[wait-what](https://github.com/mattpocock/skills/blob/main/skills/productivity/wait-what/SKILL.md)**：消息没听懂的瞬间立刻开火。agent 会用你缺的上下文，用白话，按你的 `CONTEXT.md` 词汇重新讲一遍。

**Model-invoked**

- **[grilling](https://github.com/mattpocock/skills/blob/main/skills/productivity/grilling/SKILL.md)**：就计划、决策或想法不停追问用户，直到设计树的每个分支都落地。这是 `grill-me`、`grill-with-docs`、`triage`、`wayfinder` 和 `improve-codebase-architecture` 背后可复用的访谈原语。
- **[writing-for-agents](https://github.com/mattpocock/skills/blob/main/skills/productivity/writing-for-agents/SKILL.md)**：给 agent 写文档：skills、`AGENTS.md`/`CLAUDE.md`，以及任何 agent 通过指针会读到的文档。

## 译本说明

- 本文翻译自 2026-08-25 可见的仓库 README，原文会继续更新，请以 [mattpocock/skills](https://github.com/mattpocock/skills) 为准。
- 原文中的相对链接，在本文中改为指向该 GitHub 仓库的绝对链接。
- 技能名、命令和文件名保留英文，避免安装和调用时对不上。
