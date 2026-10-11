---
layout: post
title: "OpenAI DevDay 后续更新：Dot 手机化、Sol Ultrafast 与实用工作流"
date: 2026-10-11
---

DevDay 之后，最值得关注的变化是几项能力开始连起来：可以在手机上创建 Dot，让它接着已有的 Codex 工作推进，也可以请它检查和调整 Work 定时任务。与此同时，GPT‑6.1 Sol 的 Ultrafast 已开放，Decisions API 从大会时的有限预览推进到公开 beta。

对同时维护资料、文章和软件项目的人，这些更新的价值在于少重复交代背景、更方便地跟进工作。但要真正用起来，还需要分清任务运行在哪里、连接了哪些资源，以及成果如何验收。

> **核验日期：北京时间 2026 年 10 月 11 日。** 本文以 9 月 29 日 DevDay 为起点，重点整理官方 9 月 28 日至 10 月 2 日、10 月 5–9 日周报，以及 API 10 月更新。日期明确的发布与当前使用说明分别标注；操作建议和任务模板是作者整理，未逐项实测所有套餐与客户端。本文中的 Dot 指自己的常驻代理，Dots 指这项产品。

此前的[DevDay 功能与套餐指南](/blog/2026/09/30/openai-devday-2026-plans-and-guide)和[Dots / Space 工作流文章](/blog/2026/10/02/codex-dots-space-playbook)解释了首发框架。本文重点补充后续变化，不重新铺开套餐价格表。

## 一、先看变化：哪些是大会之后的新进展

| 时间 | 官方确认的变化 | 对使用者的影响 |
| --- | --- | --- |
| 10 月 5–9 日周报 | ChatGPT 手机 App 可以创建、命名和装扮 Dot，并连接插件 | 首次设置可以在手机上完成，也可选择打开 App 就进入 Dot 对话 |
| 同一周 Dot 更新 | 能搜索 ChatGPT 对话，利用已有 Codex 聊天与自动化的上下文；更好地判断续接旧任务还是新建任务 | 可以把一项正在做的工作明确交给它继续协调 |
| 同一周 Dot 更新 | 能读取和编辑 ChatGPT Work 自动化 | 可以让它核对已有安排、调整周期任务，减少重复创建 |
| 10 月 8 日 | GPT‑6.1 Sol Ultrafast 上线，覆盖符合条件的 Work / Codex 入口和 Responses API | 原先“Sol Ultrafast 后续开放”的信息需要更新 |
| 10 月 6 日 | Decisions API 使用 GPT‑6 Luna 进入公开 beta | 开发者可以试用专门的分类、条件判断和评分接口 |
| 10 月 6 日 | API 用量等级由五级简化为 Build、Launch、Grow 三级 | 使用 API 的项目需要重新核对账号和模型限额 |
| 10 月 7 日 | `chat-latest` 快照更新 | 测试聊天体验时要留意动态别名会变化 |

Dot 更新没有为每项改进单列上线日，因此表中保留官方周报的时间范围。来源：[Dot 10 月更新](https://learn.chatgpt.com/docs/whats-new/dots-october-9-2026)、[10 月 5–9 日周报](https://learn.chatgpt.com/docs/whats-new/october-5-9-2026)、[API 更新日志](https://developers.openai.com/api/docs/changelog)。

可复用的 Codex Cloud 环境、Security Cloud、跨设备 Work、Space、插件扩展和 MCP Events，则已列在大会所在周的官方汇总中。它们是理解新工作流的基础，不宜全部写成 10 月才新增的功能。[9 月 28 日至 10 月 2 日周报](https://learn.chatgpt.com/docs/whats-new/september-28-october-2-2026)

## 二、Dot 更新的重点：从随时联系，走向连续协作

### 1. 手机上可以完成首次设置

现在可在最新版 ChatGPT 手机 App 中创建 Dot，设置名字、外观并连接插件，还能选择让 App 直接打开 Dot 对话。桌面 App 和桌面浏览器也可以设置；**手机 App 与手机网页要区分，当前手机网页不支持 Dot。** [官方入门指南](https://learn.chatgpt.com/docs/dots/getting-started#create-your-dot)

实际操作可以按这个顺序：

1. 更新 ChatGPT 手机 App，在账号已开放的 Dots 入口开始设置。
2. 先给 Dot 命名，外观可随时调整。
3. 只连接第一项职责需要的插件，例如资料所在的文件服务。
4. 发出一项明确的持续职责，检查首次结果后再增加范围。

同一个 Dot 可以跨设备继续使用。我的建议是：手机负责交代优先级、补充约束和处理决定；复杂文件与代码结果尽量在方便检查的屏幕上审阅。

### 2. 更容易接着已有 Codex 工作做

本周更新明确提到，Dot 能启动 Codex 工作、跟进现有聊天，并利用 ChatGPT 对话、Codex 聊天及自动化中的相关上下文。它也更善于判断是否应该继续原来的聊天。这让“接着上次那个项目做”更有用，但任务定位仍应具体。[Dot 10 月更新](https://learn.chatgpt.com/docs/whats-new/dots-october-9-2026)

比如，把“帮我继续网站开发”改成下面这样：

```text
请继续 Codex 中名为「项目发布检查」的已有聊天。
先阅读它的目标、最近结果和未完成项，告诉我下一步是什么。
本轮只修复已经确认的问题，保留现有设计与无关改动。
如果有多个同名聊天，先让我选择。
完成后给出结果、验证证据和仍需我决定的事项。
```

这里的聊天名是示例，使用时换成实际名称。给出明确目标，能减少它续接错误项目或重复开工的机会。

**能使用相关上下文，不代表无条件读取所有应用里的所有聊天。** 当前文档说明：续接已有本地 Codex 任务需要连接相应电脑；新建云端编码任务可以使用事先准备好的 Codex 云端环境；分派出去的新任务有自己的对话，也不会自动获得你与 Dot 的全部聊天。[任务与上下文说明](https://learn.chatgpt.com/docs/dots/tasks-and-memory#assigned-work)

### 3. 可以管理已有 Work 自动化

Dot 现在可以读取、编辑 ChatGPT Work 自动化。最实用的起点是请它盘点已有安排，再修改指定任务，而不是又建一套相同的提醒。[Dot 10 月更新](https://learn.chatgpt.com/docs/whats-new/dots-october-9-2026)

```text
请列出你能访问的 ChatGPT Work 定时任务：
任务名称、执行时间与时区、目标、结果交付位置、当前状态。

把「产品更新摘要」改为每个工作日北京时间 08:30 执行。
保持原来的来源与交付位置，不创建重复任务。
只有重要变化、执行失败或需要我判断时才通知。
不要恢复任何已暂停任务。
修改后确认保存的时间、状态和任务内容。
```

这是请求修改具体任务的模板。实际使用前先核对任务名称和范围，避免把“整理一下”解释成停用或重建一批安排。

固定时间的工作需要**真正保存的调度**。让 Dot 口头答应“每天检查”，或者在 Page 里写“每日更新”，都不能作为调度已建立的证据；应让它确认保存结果，并在 **Scheduled** 中检查时间、时区和交付位置。[周期任务文档](https://learn.chatgpt.com/docs/dots/tasks-and-memory#recurring-tasks)

### 4. 日常体验也有一轮修复

本周更新还包括云端浏览与侧边栏加载加速、减少正在使用 ChatGPT 时的多余手机通知，以及 Outlook 设置卡住、邮件验证码自动填充、网页代码块和附件显示等改进。失败活动可以重试加载，审批按钮被大 Gmail 预览遮挡的问题也得到修复。[完整改进列表](https://learn.chatgpt.com/docs/whats-new/dots-october-9-2026#other-improvements)

这些修复会改善协调体验，但不能据此断言所有任务都更快完成。资料访问、网站验证、电脑在线状态和实际验收仍会影响结果。

## 三、Dot 怎么用：先交一项职责，再扩成工作流

Chat 适合问答与短稿，Work 适合交付一项可检查的工作，Codex 适合软件开发，Dot 适合在对话之间持续跟进一项责任。它们可以配合使用。[官方产品分工](https://learn.chatgpt.com/docs/dots#how-dots-compare-with-chatgpt-work-and-codex)

我的起步建议是给 Dot 写清五件事：**负责什么、依据什么、何时检查、产出放哪里、什么情况找你。** 先选一项范围小、容易核验的职责，例如持续维护指定产品的更新记录。

```text
未来两周，帮我维护一份指定产品的更新记录。
仅使用我列出的官方发布日志与文档。
每个工作日北京时间 08:30 检查，结束日期由我明确指定。
发现新内容后，更新指定 Page；没有 Page 权限时先说明阻塞。

每项记录包含：发布日期、功能变化、适用条件、使用方法和来源。
保留旧结论；发生变化时注明旧版信息被什么替代。
没有重要变化时不发提醒；发现重大更新、来源访问失败、
或需要我判断时，在当前 ChatGPT 对话里通知我。
先确认结束日期和交付位置，再保存调度并回报。
```

模板需要填写自己的产品、来源、Page 和结束日期。不要把它原样交给代理后期待自动猜齐所有条件。

对多项目工作，我会采用以下分工：Dot 跟进变化、缺失资料和待办；Work 整理研究、表格与文稿；Codex 实现和验证软件；Page 或本地文件保存稳定成果。这是使用建议，具体能否执行取决于连接和权限。Space 的 Page 可以通过 `@ChatGPT` 或 `@dot` 请求协助，相关能力已见于大会所在周的官方说明。[Space 与 Pages 使用入口](https://learn.chatgpt.com/docs/whats-new/september-28-october-2-2026#work-together-in-chatgpt-space)

例如，教学项目可以让 Dot 跟踪待补资料，由 Work 形成课件初稿、Codex 改进互动工具；信息整理项目可以让 Dot 维护来源变化，把事实核验和观点分析交给具体任务；个人博客可以由 Dot 跟进选题与资料，在获得明确发布授权后交给相应工作流上线。

首次试用时，我会重点检查三件事：是否找对来源、是否保留原有决定、遇到失败是否如实报告。只读资料失败就写“暂无更新”，会让持续跟踪失去意义。

## 四、运行位置、权限与停止方式，要一起弄清

### 云端继续工作，本地资源仍依赖电脑

Dot 有自己的云端电脑和浏览器，因此不依赖你的电脑完成的云端工作可以继续运行。若要操作你电脑上的文件、应用或已有本地 Codex 任务，则需要连接那台电脑，并保持电脑在线、ChatGPT App 打开。[电脑连接指南](https://learn.chatgpt.com/docs/dots/getting-started#connect-your-computer)

| 需求 | 要确认的条件 |
| --- | --- |
| 云端查网页、整理云端资料 | 所需网站、插件及账号访问可用 |
| 手机发指令，继续已有本地 Codex 工作 | 对应电脑已连接且在线，任务名称与目标明确 |
| 电脑关机时继续云端编码 | 已在 Codex 准备相应仓库和云端环境 |
| 跨设备继续 Work | 工作区启用了对应同步能力，新任务使用适用模式；本地步骤仍需电脑在线 |

切换 Dot 选中的电脑，不会自动把已有任务搬到另一台电脑。跨设备 Work 也有独立设置，已有任务保留原先模式；不要把“手机能发消息”当成“所有本地任务都已迁移到云端”。[Dot 任务位置](https://learn.chatgpt.com/docs/dots/tasks-and-memory#assigned-work)、[跨设备 Work](https://learn.chatgpt.com/docs/get-started-with-work#continue-work-across-devices)

### 连接渠道、插件和电脑是三件事

Slack / Teams 等渠道负责联系与通知；插件提供相应服务的数据与工具；电脑连接提供本地资源。接通一个渠道，不会同时授予邮箱、其他应用或电脑权限。[Dot 连接说明](https://learn.chatgpt.com/docs/dots#connections)

授权也要与动作对应。请它起草回复，只授权起草；请它跟踪资料，不等于授权对外发布。可以在对话中直接规定范围，也可以在 **Settings → Personalization → Permissions → Custom rules** 设置特定动作的处理方式，例如客户消息必须先审阅。[权限与自定义规则](https://learn.chatgpt.com/docs/dots/controls#set-custom-rules)

### 暂停、停止分派任务、取消调度分别处理

在桌面端 Dot 资料页的 **Activity** 查看分派任务、成果和等待处理的请求，在 **Scheduled** 查看周期任务。**Pause** 暂停当前主任务，不会停止所有分派工作，也不会取消未来调度；要结束对应工作，应分别检查 Activity 和 Scheduled。结束语音通话也不代表任务已经停止。[停止工作说明](https://learn.chatgpt.com/docs/dots/controls#stop-work)

当前 Dot 文档列出的开放范围包括 Pro 100 / 200 / 500、Business Premium 和 Enterprise，并继续分批开放。个人 Pro 要求年满 18 岁，且不在欧洲经济区、英国和瑞士；Business Premium、Enterprise 全球逐步开放，Enterprise 默认关闭、需管理员开启。套餐符合条件，也可能暂时看不到入口。[当前可用性](https://learn.chatgpt.com/docs/dots#access)

Dot 对话本身不计入 ChatGPT 用量限制，但它启动或管理的 Work / Codex 任务照常消耗相应用量。持续跟进的便利仍需要结合任务成本评估。[Dot 用量说明](https://learn.chatgpt.com/docs/dots#access)

## 五、GPT‑6.1 Sol Ultrafast：现在能用，但要看入口与成本

9 月 30 日的文章还写着 Sol Ultrafast 将后续开放；**10 月 8 日这一状态已改变**。官方周报确认，GPT‑6.1 Sol Ultrafast 可用于 Codex 和 ChatGPT Work，适用 Pro 500 以及符合条件的 Enterprise / Edu；Enterprise 默认关闭，需要工作区负责人开启。[10 月 5–9 日周报](https://learn.chatgpt.com/docs/whats-new/october-5-9-2026)

使用时先在支持的入口选择 GPT‑6.1 Sol，再选择账号提供的 Ultrafast 速度档位，并检查用量。没有入口时先核对套餐、客户端和管理员设置；其他自助套餐在当前开放范围内，即使购买 credits 也不会因此解锁 Ultrafast。[速度模式文档](https://learn.chatgpt.com/docs/agent-configuration/speed#ultrafast-mode)

### 消耗倍率与速度提升分开看

| 速度档位 | 套餐内额度消耗，相对同模型 Standard | 购买 credits / Enterprise 按量消耗 |
| --- | --- | --- |
| Fast | 2.5 倍 | 2 倍 |
| Ultrafast | 8 倍 | 6 倍 |

这些是用量或计费倍率。官方“最高 8 倍 token 生成速度”描述针对 Astra，不能直接移用为 Sol 的速度承诺，更不能理解成整个任务耗时缩短 8 倍。当前 Sol Ultrafast 支持美国及欧洲（EEA + 瑞士）推理驻留，Astra Ultrafast 支持美国推理驻留。[速度与计费规则](https://learn.chatgpt.com/docs/agent-configuration/speed)

我的建议是：整理资料、后台批处理先用 Standard；人在屏幕前连续调试、生成等待确实影响效率时，再评估 Ultrafast。搜索、工具执行和网站等待占主要时间的任务，提速收益可能有限。

### API 是另一套资格与账单

Responses API 在 10 月 8 日加入 Sol Ultrafast，API 用户可以使用，但受独立速率限制和 API 价格约束；这与 ChatGPT 订阅是否解锁 Ultrafast 分别判断。[API 更新日志](https://developers.openai.com/api/docs/changelog)

下面是按官方接口整理的最小请求示例，未在本文中实际调用计费 API。配置 API Key 后可用于理解参数，投入项目时再按当前 SDK、价格与限额验证：

```python
from openai import OpenAI

client = OpenAI()
response = client.responses.create(
    model="gpt-6.1-sol",
    service_tier="ultrafast",
    input="请用三句话解释什么是持续工作代理。",
)
print(response.output_text)
```

工具调用频繁的代理应用，官方建议使用持久 WebSocket 连接降低请求间的网络开销。API Key 模式走 API token 计费，不能套用上表的 ChatGPT credits 倍率。[API Ultrafast 使用指南](https://developers.openai.com/api/docs/guides/ultrafast-mode)

## 六、开发者的新进展：Decisions API 公开 beta

10 月 6 日，Decisions API 使用 `gpt-6-luna` 进入公开 beta。它面向文本、图片的判定任务，通过独立的 `POST /v1/decisions` 返回可供程序使用的答案。[发布日期](https://developers.openai.com/api/docs/changelog)、[接口指南](https://developers.openai.com/api/docs/guides/decisions)

三类问题分别是：

| 类型 | 适合的任务 | 返回内容 |
| --- | --- | --- |
| `predicate` | 判断条件是否成立 | 条件成立的估计概率 |
| `choice` | 在预设类别中选择 | 所提供选项中的一个类别及相关概率 |
| `score` | 按有序等级评分 | 根据等级概率加权的分数 |

使用时提供 `model`、共同证据 `input` 和问题列表 `questions`，先在 Playground 比较真实样本，再接到应用中。需要自定义字段提取、完整说明或调用工具的任务，应考虑 Responses API 的结构化输出或函数调用。[问题类型与使用边界](https://developers.openai.com/api/docs/guides/decisions#choose-a-question-type)

对信息聚合项目，我会尝试让它先将条目分到固定栏目，再交给复杂模型核验事实、合并重复内容和撰写摘要。官方“约快 10 倍”描述针对这类判定接口的比较，不能推导成新闻收集与文章发布全流程快 10 倍；概率输出也不等于事实已经核实。[Decisions 官方说明](https://developers.openai.com/api/docs/guides/decisions)

API 同期还把用量等级简化为 Build、Launch、Grow，并更新了 `chat-latest` 快照。维护脚本时应重新查看限额；生产任务需要稳定比较质量时，谨慎使用会持续变化的聊天别名。[API 更新日志](https://developers.openai.com/api/docs/changelog)

## 七、接下来怎么采用

对个人工作流，我会按这个顺序试：

1. **先更新手机 App，确认 Dot 入口。** 不为尚未开放的能力提前假定购买结果。
2. **只交一项持续职责。** 用一至两周检查来源、通知频率和成果质量。
3. **盘点已有任务与调度。** 续接已有工作，核对保存结果，减少重复安排。
4. **明确云端与本地依赖。** 需要电脑的步骤保持在线；需要离线继续的编码任务先准备云端环境。
5. **按实际瓶颈采用提速或新 API。** 观察耗时、用量和人工返工，再扩大使用。

还有一个近期维护节点：**GPT‑5.5 将于 2026 年 10 月 14 日从 ChatGPT、Work 和使用 ChatGPT 登录的 Codex 退休，OpenAI API 不受这一退休安排影响。** 仍依赖它的用户应检查任务默认模型、自动化与保存的配置，选择账号实际可用的替代模型。[退休说明](https://learn.chatgpt.com/docs/agent-configuration/speed#retirement-and-migration)

对我来说，这轮更新最有参考价值的是连续工作更容易落地了：手机上能交代和调整方向，已有聊天能被明确续接，定时安排能被检查和修改。把来源、资源、交付和停止条件写清，再检查实际结果，才是把新功能变成日常工作能力的关键。
