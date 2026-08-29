---
layout: post
title: "Matt Pocock Skills 全景指南：37 个技能与工程工作流"
date: 2026-08-29
---

Matt Pocock 的 [Skills For Real Engineers](https://github.com/mattpocock/skills) 不是一套接管开发流程的庞大框架，而是一组可以单独使用、按需组合的 agent skills。它们把需求澄清、领域建模、规格拆解、测试驱动开发、代码审查和上下文交接等工程实践，封装成可重复调用的小型工作单元。

本文基于仓库提交 [`6654f6b`](https://github.com/mattpocock/skills/commit/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76) 进行整理，共盘点 37 个 `SKILL.md`：25 个正式技能、4 个非主推工具和 8 个公开测试版。每个技能只保留核心功能和典型使用场景，重点说明它们在一条真实工程工作流里如何接力。

> 这是一份固定提交快照。上游仓库仍在更新，技能的状态、名称和行为应以最新版本为准。

## 一、先看懂两条分类轴

### 1. 成熟度：正式、非主推与测试版

- **Engineering（18 个）**：作者日常用于代码工程的正式技能。
- **Productivity（7 个）**：不局限于编码的正式工作流工具。
- **Misc（4 个）**：作者保留但很少使用的专用工具，不随正式插件推广。
- **In Progress（8 个）**：公开征求反馈的测试版，不包含在正式插件和顶层 README 中，可能改变或消失。

Claude Code 插件的 [`skills` 清单](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/.claude-plugin/plugin.json) 只包含 Engineering 与 Productivity 两组，共 25 个正式技能。

### 2. 调用方式：用户编排与模型纪律

根据上游 README 的 [Reference 分类说明](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/README.md#reference)，正式技能还可以按“谁能调用”分成两类：

- **User-invoked**：只有用户明确输入技能名时才能启动，主要负责选择路线和编排流程。正式技能中有 14 个属于这一类。
- **Model-invoked**：用户可以明确调用，模型也可以在任务匹配时自动使用，主要承载可复用的工程纪律。正式技能中有 11 个属于这一类。

仓库约定，user-invoked skill 可以调用 model-invoked skill，但不应再调用另一个 user-invoked skill。理解这条边界后，会更容易看清为什么 `implement` 可以在内部使用 `tdd` 和 `code-review`，而流程之间的切换仍由人控制。

## 二、正式 Engineering skills（18 个）

| Skill | 核心功能与典型使用场景 |
| --- | --- |
| [`ask-matt`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/ask-matt/SKILL.md) | 整套技能的路线导航器。不知道该从哪个 skill 或哪条流程开始时，用它根据当前局面选入口。 |
| [`setup-matt-pocock-skills`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/setup-matt-pocock-skills/SKILL.md) | 配置 issue tracker、triage 标签和领域文档布局。每个仓库第一次使用这套工程技能前运行一次。 |
| [`grill-with-docs`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/grill-with-docs/SKILL.md) | 通过持续访谈把方案问清楚，同时维护领域词汇表和 ADR。适合仓库内的新功能、设计和重要决策。 |
| [`triage`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/triage/SKILL.md) | 用状态机分类、验证外部 issue 或 PR，补齐信息并产出 agent-ready brief。适合处理别人提交的原始请求。 |
| [`improve-codebase-architecture`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/improve-codebase-architecture/SKILL.md) | 扫描浅模块和薄弱边界，以 HTML 报告展示可以“加深模块”的候选项。适合定期架构体检，而不是紧急救火。 |
| [`to-spec`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/to-spec/SKILL.md) | 把已经讨论清楚的上下文综合成可实现规格并发布到 tracker。适合决策已经完成、需要固化方案时。 |
| [`to-tickets`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/to-tickets/SKILL.md) | 把计划或规格拆成声明阻塞关系的 tracer-bullet 垂直切片。适合多会话、多人或多 agent 实施。 |
| [`implement`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/implement/SKILL.md) | 根据 spec 或 tickets 编码，在约定接缝上使用 TDD，并以代码审查和提交收尾。适合需求已经决策完整的实现任务。 |
| [`wayfinder`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/wayfinder/SKILL.md) | 把超出单次会话容量的模糊项目绘制成“决策票据地图”，逐个消除未知。适合大型绿地项目或巨型功能。 |
| [`prototype`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/prototype/SKILL.md) | 用一次性代码回答一个设计问题。适合验证状态模型、业务逻辑手感，或比较几个差异明显的 UI 方向。 |
| [`diagnosing-bugs`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/diagnosing-bugs/SKILL.md) | 按“反馈闭环、最小复现、假设、插桩、修复、回归测试”诊断问题。适合顽固 bug、偶发失败和性能回退。 |
| [`research`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/research/SKILL.md) | 让后台 agent 基于一手资料调研，并在仓库中留下带引用的 Markdown。适合核对 API、规范、源码和技术选型事实。 |
| [`tdd`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/tdd/SKILL.md) | 通过红、绿、重构循环，一次交付一个可观察的垂直切片。适合测试先行地实现功能或修复缺陷。 |
| [`domain-modeling`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/domain-modeling/SKILL.md) | 校准领域术语、`CONTEXT.md` 和 ADR。适合术语含糊、一个词承担多重含义，或重要决策需要持久化时。 |
| [`codebase-design`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/codebase-design/SKILL.md) | 提供深模块、接口、接缝、适配器、局部性等统一设计词汇。适合讨论模块边界、可测试性和 agent 可导航性。 |
| [`code-review`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/code-review/SKILL.md) | 相对固定基点，从 Standards 与 Spec 两个维度并行审查差异。适合检查分支、PR 或一组已完成改动。 |
| [`resolving-merge-conflicts`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/resolving-merge-conflicts/SKILL.md) | 追溯冲突双方的一手意图，逐个 hunk 解决正在进行的 merge 或 rebase，并完成该操作。 |
| [`wizard`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/wizard/SKILL.md) | 为只有人类能完成的凭据配置、后台点击、迁移或切换步骤生成交互式 Bash 向导。 |

## 三、正式 Productivity skills（7 个）

| Skill | 核心功能与典型使用场景 |
| --- | --- |
| [`grill-me`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/productivity/grill-me/SKILL.md) | 通过连续提问把计划或想法的每个决策分支问清楚，但不维护仓库文档。适合没有工作目录的规划、设计或写作。 |
| [`handoff`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/productivity/handoff/SKILL.md) | 把当前上下文压缩成可移交的 Markdown。适合切换 agent、目录、执行环境，或把工作交给同事。 |
| [`teach`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/productivity/teach/SKILL.md) | 把当前目录作为持续学习工作区，通过课程、资料和学习记录教授概念或技能。适合跨会话学习。 |
| [`to-questionnaire`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/productivity/to-questionnaire/SKILL.md) | 把只有其他利益相关者能回答的未知项整理成 Markdown 问卷。适合异步确认业务规则、政策或组织决策。 |
| [`wait-what`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/productivity/wait-what/SKILL.md) | 当上一段解释没有讲明白时，补足缺失背景，并使用项目已有词汇重新表述。 |
| [`grilling`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/productivity/grilling/SKILL.md) | 可复用的“穷尽决策分支”访谈原语。它是 `grill-me`、`grill-with-docs`、`triage`、`wayfinder` 等上层技能的共同底座。 |
| [`writing-for-agents`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/productivity/writing-for-agents/SKILL.md) | 指导编写给 agent 使用的文档，包括 skills、`AGENTS.md`、`CLAUDE.md` 和被这些文件引用的说明。 |

## 四、Misc 非主推工具（4 个）

这些工具公开保留在仓库里，但不会随正式插件安装。它们的使用面较窄，更像针对特定技术栈或工作环境的实用脚本。

| Skill | 核心功能与典型使用场景 |
| --- | --- |
| [`git-guardrails-claude-code`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/misc/git-guardrails-claude-code/SKILL.md) | 配置 Claude Code hooks，在执行前阻止 push、`reset --hard`、clean、强制删分支等危险 Git 命令。 |
| [`migrate-to-shoehorn`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/misc/migrate-to-shoehorn/SKILL.md) | 将 TypeScript 测试 fixture 中的 `as` 类型断言迁移到 `@total-typescript/shoehorn`。只适用于测试代码。 |
| [`scaffold-exercises`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/misc/scaffold-exercises/SKILL.md) | 为课程生成 section、problem、solution、explainer 等练习目录和可通过 lint 的占位文件。 |
| [`setup-pre-commit`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/misc/setup-pre-commit/SKILL.md) | 配置 Husky、lint-staged、Prettier、类型检查和测试组成的提交前质量门。适合 JavaScript 或 TypeScript 仓库。 |

## 五、In Progress 公开测试版（8 个）

测试版不会出现在正式插件或顶层 README 中，需要通过 `skills.sh` 单独安装。它们可能随时改变或消失，不应当成稳定工作流依赖。

| Skill | 核心功能与典型使用场景 |
| --- | --- |
| [`loop-me`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/in-progress/loop-me/SKILL.md) | 在多次会话中持续访谈，把反复发生的生活或工作循环整理成可实施的 workflow 规格。 |
| [`claude-handoff`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/in-progress/claude-handoff/SKILL.md) | 通过 `claude --bg` 把当前对话直接交给新的 Claude 后台 agent。适合需要立即续做的 Claude Code 场景。 |
| [`implement-spec`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/in-progress/implement-spec/SKILL.md) | 把整份规格视作任务图，并行调度当前没有阻塞的任务，最后合并成一个 PR。适合测试并发实现模式。 |
| [`retro`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/in-progress/retro/SKILL.md) | 目标是复盘编码会话并改进 agent 环境、标准、检查和信息访问；当前仍是设计草案，尚不可正常使用。 |
| [`setup-ts-deep-modules`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/in-progress/setup-ts-deep-modules/SKILL.md) | 用 dependency-cruiser 强制 TypeScript 包只能通过根入口访问，并隐藏内部实现。适合试验深模块约束。 |
| [`writing-beats`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/in-progress/writing-beats/SKILL.md) | 按叙事或论证的 beat 逐步选路，先建立读者需要的概念，再推进下一段。适合交互式长文写作。 |
| [`writing-fragments`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/in-progress/writing-fragments/SKILL.md) | 在不确定结构时挖掘并积累异质原始片段。适合文章的素材探索阶段。 |
| [`writing-shape`](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/in-progress/writing-shape/SKILL.md) | 把固定的一组原始材料逐段塑造成文章，并逐步确认组织选择。适合素材已经齐全的收敛阶段。 |

## 六、这些技能如何组成工作流

### 1. 主流程：从想法到交付

根据 [`ask-matt` 的完整路线图](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/ask-matt/SKILL.md) 与配套的 [阶段边界规则](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/ask-matt/PHASE-BOUNDARIES.md)，绝大多数功能开发都可以沿着下面这条主线推进：

```text
每个仓库首次使用
setup-matt-pocock-skills
        │
        ▼
需求澄清
grill-with-docs
        │
        ├─ 缺外部事实：research ───────────────┐
        ├─ 答案掌握在别人手中：to-questionnaire ┤
        └─ 需要运行后才能判断：                  │
           handoff → prototype → handoff ──────┘
        │
        ▼
规模判断
        ├─ 单会话可完成：implement
        │
        └─ 需要多会话：to-spec → to-tickets → implement
                                                │
                                                └─ tdd → code-review
```

这里有三个容易混淆的点：

1. `to-spec` 只综合已经谈清楚的内容，不负责继续访谈。仍有未知时，应先回到 `grill-with-docs`，或用 `research`、`to-questionnaire`、`prototype` 补证据。
2. `prototype` 的产物用于回答设计问题，不是生产实现。得到的结论需要回流到原来的讨论或规格。
3. `implement` 是有明显副作用的技能：它会修改代码、运行检查、审查差异并提交当前分支，只有在任务已经可实现时才应启动。

### 2. 三条常用入口支线

| 当前局面 | 推荐链路 | 为什么这样接入 |
| --- | --- | --- |
| 外部 bug 报告或功能请求不断积压 | `triage` → agent-ready issue → `implement` | 先验证和补齐别人提交的原始信息，再进入实现。 |
| 遇到顽固 bug、偶发失败或性能回退 | `diagnosing-bugs` → 回归测试或 `tdd` | 先建立能稳定变红的反馈闭环，再谈假设与修复。 |
| 项目大到单次会话看不清全貌 | `wayfinder` → `to-spec` → `to-tickets` → `implement` | `wayfinder` 产出决策地图而不是代码，路径清晰后仍需压缩成正式规格。 |

`to-tickets` 生成的 tickets 已经是 agent-ready，不应再交给 `triage`。`triage` 只处理来自外部、尚未整理的 issue 或 PR。

### 3. 贯穿流程的三种支撑能力

- `domain-modeling` 负责领域语言：让需求、规格、代码命名和 ADR 使用同一套词汇。
- `codebase-design` 负责模块形状：用深模块、接口、接缝和局部性等概念支撑 TDD 与架构改进。
- `handoff` 负责真正的边界切换：换 agent、换目录、换执行器或交给同事时再使用，不必在同一阶段频繁生成交接文档。

### 4. 架构维护与写作实验

架构维护不是功能主流程的一部分。可以定期运行 `improve-codebase-architecture` 找候选，再把选中的改进点送回 `grill-with-docs`，沿主流程完成设计与实现。

测试版写作技能则形成另一条清晰链路：

```text
writing-fragments
        │
        ├─ writing-shape：按段落组织固定素材
        └─ writing-beats：按叙事或论证节拍逐步推进
```

`retro` 未来可能接在一次编码会话结束之后，用来改进 steering 文件、编码标准和自动检查；但在当前快照中，它仍只是未完成的设计说明。

## 七、选技能时最实用的判断规则

下面的快速判断同样来自 [`ask-matt` 路由](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/ask-matt/SKILL.md)，用于把当前局面映射到合适入口：

- 不知道从哪里开始，先用 `ask-matt`。
- 在仓库里澄清需求，用 `grill-with-docs`；没有工作目录时，才用不留文档的 `grill-me`。
- 缺权威资料，用 `research`；缺利益相关者的答案，用 `to-questionnaire`；必须运行后才知道，用 `prototype`。
- 工作量能装进一个会话，直接进入 `implement`；需要跨会话，先 `to-spec` 和 `to-tickets`。
- `wizard` 只处理 agent 确实不能代办的人类步骤。如果 agent 可以直接完成，就不应该把工作推回给人。
- `resolving-merge-conflicts` 只在 merge 或 rebase 已经发生冲突时使用。它按双方意图完成冲突解决，不会选择 `--abort`。
- `misc` 与 `in-progress` 都不属于正式插件能力。前者是低频专用工具，后者还承担变化或失效风险。

## 八、安装与首次配置

上游 README 的 [Installation 说明](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/README.md#installation-30-second-setup) 提供两种安装思路，应该二选一。两种方式同时使用，会让每个 skill 出现两份。

**Claude Code 托管插件：**

```bash
claude plugins install mattpocock-skills
```

插件是只读的受管理包，作者发布更新后会自动到达。

**Codex、Claude Code 或其他兼容 agent 的可编辑副本：**

```bash
npx skills@latest add mattpocock/skills
```

这种方式会把选中的 skill 文件复制到项目中，方便自行修改；需要更新时再主动运行 `npx skills update`。安装时应确保选中 `setup-matt-pocock-skills`，然后在每个仓库里运行一次：

```text
/setup-matt-pocock-skills
```

它会确认 tracker、triage 标签和文档存放位置，为后续工程工作流补齐前置配置。

## 九、总结

这套 skills 最有价值的地方，不是单个命令有多聪明，而是把工程活动拆成职责清楚的阶段：先通过访谈、研究和原型减少误解，再把决策沉淀为规格与 tickets，最后依靠 TDD、代码审查和反馈闭环完成交付。

实际使用时不必一次安装或记住全部 37 个。可以先从 `setup-matt-pocock-skills`、`ask-matt`、`grill-with-docs`、`implement` 和 `diagnosing-bugs` 建立主干，再根据项目规模逐步加入 `to-spec`、`to-tickets`、`wayfinder` 和架构类技能。测试版和非主推工具则应按具体需求单独评估。

## 参考资料

- [仓库顶层 README](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/README.md)
- [Claude Code 正式插件清单](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/.claude-plugin/plugin.json)
- [`ask-matt` 完整工作流路由](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/ask-matt/SKILL.md)
- [`ask-matt` 阶段边界规则](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/ask-matt/PHASE-BOUNDARIES.md)
- [Engineering skills 清单](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/engineering/README.md)
- [Productivity skills 清单](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/productivity/README.md)
- [Misc skills 说明](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/misc/README.md)
- [In Progress skills 说明](https://github.com/mattpocock/skills/blob/6654f6b60cd9d5be8b54c6fafe44346dabeb3b76/skills/in-progress/README.md)
