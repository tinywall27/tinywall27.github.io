---
layout: post
title: "Codex Plus 模型额度优化：Sol 统筹、Luna 执行"
date: 2026-09-06
---

> 调研与配置日期：2026-09-06

## 调研问题

如何为 Codex Plus 用户合理分配模型任务，在保留复杂任务质量的同时减少 GPT-5.6 Sol 额度消耗：由 Sol 负责规划、高难度推理和最终验收，并把清晰、简单、可独立执行的工作交给 GPT-5.6 Luna 子代理。

## 结论

“Sol 统筹、Luna 执行”符合 OpenAI 对两个模型的官方定位，也与 X 上常见的 Codex 多代理实践一致：Sol 适合模糊、多步骤、需要规划、工具调用和验证的工作；Luna 适合快速、范围窄、清晰、可重复或高吞吐的工作。

这一方案主要节省稀缺的 Sol 使用额度，并不保证减少总 token 消耗。每个子代理都有独立的上下文、模型调用和工具调用，因此多代理通常比可比的单代理执行消耗更多 token。只有当子任务独立、边界明确，而且省下的 Sol 工作量足以覆盖协调成本时，才值得委派。

最佳实践可以归纳为：

- Sol 保留规划、歧义消解、架构设计、复杂推理、跨模块决策、集成和最终验证。
- Luna 承担定向文件查找、信息提取、机械性修改、格式整理和常规检查等明确、低风险任务。
- 不为琐碎的一步操作启动子代理，也不并行修改相同文件。
- 每个子代理只接收一个有明确范围、输入输出、验收标准和停止条件的任务。
- 主代理使用子代理的精炼结果，只复核真正影响风险和正确性的部分，避免重复劳动。

## 最终修改

### `~/.codex/AGENTS.md` 英文原文

```markdown
## Model routing and delegation

- Keep planning, ambiguity resolution, architecture, difficult reasoning, cross-cutting decisions, integration, and final verification in the parent agent. Prefer GPT-5.6 Sol for these responsibilities.
- Delegate only independent, bounded work whose expected savings in parent-model usage justify the additional context and coordination cost. Do not spawn a subagent for trivial one-step work or tightly coupled changes.
- For clear, repeatable, low-risk execution—targeted file discovery, focused extraction, mechanical edits, formatting, and routine checks—use `gpt-5.6-luna` with `low` reasoning effort.
- Give each subagent one concrete objective, exact scope and inputs, required output, acceptance criteria, and stop conditions. Require a concise result instead of raw logs.
- Keep ambiguous debugging, architectural or security judgment, overlapping cross-file edits, conflict resolution, and final acceptance in the parent agent. Escalate work back to the parent when requirements become unclear or the task expands beyond its assigned scope.
- Prefer parallel delegation for substantial independent read-heavy work. Avoid parallel agents that would edit the same files or depend on each other's unfinished results.
- Do not repeat a subagent's work. Review its result and independently verify only the risk-bearing parts.
```

### `~/.codex/AGENTS.md` 中文翻译

```markdown
## 模型路由与任务委派

- 将规划、歧义消解、架构设计、高难度推理、跨模块决策、集成和最终验证保留给主代理。优先使用 GPT-5.6 Sol 承担这些职责。
- 仅委派独立且边界明确的工作，并确保预期节省的主模型用量足以抵消额外的上下文和协调成本。不要为琐碎的一步任务或紧密耦合的改动启动子代理。
- 对于清晰、可重复、低风险的执行工作——例如定向文件查找、聚焦式信息提取、机械性修改、格式整理和常规检查——使用 `gpt-5.6-luna`，并将推理强度设为 `low`。
- 为每个子代理提供一个具体目标、准确的范围和输入、要求的输出、验收标准以及停止条件。要求返回精炼结论，而不是原始日志。
- 将存在歧义的调试、架构或安全判断、重叠的跨文件修改、冲突解决和最终验收保留给主代理。当需求变得不清楚，或任务超出已分配范围时，将工作升级回主代理。
- 对工作量较大、彼此独立且以读取为主的任务，优先采用并行委派。避免让多个并行代理修改相同文件，或依赖彼此尚未完成的结果。
- 不要重复子代理已经完成的工作。审阅其结果，并且只独立验证真正影响风险的部分。
```

### `~/.codex/config.toml`

主模型原本已经配置为 Sol，因此保留：

```toml
model = "gpt-5.6-sol"
model_reasoning_effort = "medium"
```

新增以下全局子代理默认配置：

```toml
[agents]
default_subagent_model = "gpt-5.6-luna"
default_subagent_reasoning_effort = "low"
```

`AGENTS.md` 负责规定模型分工和委派边界，`config.toml` 负责真正设置主模型及子代理默认模型。两者配合后，新启动的 Codex 任务会以 Sol 作为主模型，并在启动未显式指定模型的子代理时默认使用 Luna Low。显式指定的子代理模型或推理强度仍会覆盖这些默认值。

## 验证结果

- 全局 `AGENTS.md` 已写入上述英文规则。
- 不存在会覆盖它的全局 `AGENTS.override.md`。
- `config.toml` 已通过 TOML 解析与目标字段断言校验。
- 现有插件、沙箱、桌面和项目配置均未改动。
- `AGENTS.md` 在任务启动时读取，因此应新建任务或重启 Codex 会话以完整应用新规则。

## 参考资料

- [OpenAI Developers 在 X 上发布的 Codex 子代理介绍](https://x.com/OpenAIDevs/status/2033637455136731431)
- [OpenAI 官方文档：Subagents](https://learn.chatgpt.com/docs/agent-configuration/subagents)
- [OpenAI 官方文档：Custom instructions with AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md)
- [OpenAI 官方文档：Configuration Reference](https://learn.chatgpt.com/docs/config-file/config-reference)
- [OpenAI 官方文档：Codex pricing](https://learn.chatgpt.com/docs/pricing)
