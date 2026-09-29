---
layout: post
title: "OpenAI DevDay 2026：新功能、订阅套餐比较与使用建议"
date: 2026-09-30
---

OpenAI DevDay 2026 最值得关注的变化，是 AI 开始承担持续工作：dots 跟进长期任务，Space 和 Pages 沉淀协作成果，Codex 在云端运行，而 ChatGPT 订阅额度可以接入部分第三方工具。个人用户可以先从 Plus 和 GPT‑6.1 Sol 开始验证自己的工作流，再根据实际用量考虑 Pro；Pro 500 更适合明确需要 Astra Ultrafast、且时间收益足以覆盖月费的人。

> **核验日期：北京时间 2026 年 9 月 30 日。** 本文依据官方发布、产品文档和帮助中心整理，使用建议是作者判断，并非逐项实测报告。新功能正在分批开放，套餐、地区、客户端和管理员设置都可能影响可用性；价格以美元官方标价为基准，实际税费、币种及购买渠道以结算页为准。

## 一、先分清：大会发布了什么，哪些还要等

大会于旧金山当地时间 9 月 29 日举行。官方回顾列出了 20 多项发布，涵盖模型、代理、开发工具、协作和订阅生态。本文重点解释会影响日常使用与购买决策的内容。[大会官网](https://devday.openai.com/)、[官方大会回顾](https://openai.com/index/devday-2026-recap/)

### 1. GPT‑6.1 Sol：优先试用的新主力模型

GPT‑6.1 Sol 升级了编程、电脑操作和专业工作能力。官方报告称，它在部分评测中接近 Astra，但标准 API 输入、输出单价只有 Astra 的五分之一。这是特定评测和价格的比较，不能直接理解为所有任务都能得到相同质量、节省相同比例的总成本。

**首发入口是 ChatGPT Work、Codex 和 API，普通 Chat 尚未开放。** Plus、Pro、Business、Enterprise 和 Edu 用户可在支持的入口使用；API 模型名为 `gpt-6.1-sol`，每百万 token 标准价格如下：

| 输入 | 缓存输入 | 输出 |
| --- | --- | --- |
| $2 | $0.10 | $10 |

这些是 API 价格，不能据此换算订阅能完成多少次任务。[GPT‑6.1 Sol 官方发布](https://openai.com/index/introducing-gpt-6-1-sol/)

我的建议是：先用 6.1 Sol 处理日常开发、资料调研和复杂文档工作；把 Astra 留给困难推理、复杂架构和反复解决不了的问题。批量提取、格式整理等边界清楚的任务仍可试用 Luna，再检查准确性。Sol、Luna 和 Astra 属于不同能力与成本定位；此前的 GPT‑6 Sol / Luna 发布也不要混写成这次大会才首次推出。[GPT‑6 Sol 与 Luna 官方介绍](https://openai.com/index/introducing-gpt-6-sol-and-luna/)

### 2. dots：能持续跟进工作的常驻代理

dots 由 GPT‑6 Astra 驱动，拥有自己的云端电脑，可以结合已连接的应用持续工作。Pro 和 Business Premium 在符合条件的市场逐步开放；Enterprise、Edu 和 Healthcare 需由管理员开启 beta。

首个 dot 随符合条件的套餐提供，与 dot 的对话不计入 ChatGPT 使用限制；但 dot 启动或管理的 Codex / Work 任务仍按相应额度计入。**“全天在线”不代表后台工作无限量。** [dots 官方发布与用量说明](https://openai.com/index/introducing-dots/)

适合的起点是一项小而持续的职责，例如“跟踪指定产品的官方更新，有重大变化才整理摘要”。给它固定来源、输出格式、通知条件和权限边界，再逐渐扩大职责。发布文章、发送邮件和修改共享资料等动作，应该在任务指令中明确何时允许执行、何时需要审阅。

### 3. Space、Pages：把聊天成果变成持续维护的资料

Space 为 Pro、Business 和 Enterprise 提供资料与协作空间，Pages 支持与队友、ChatGPT 或 dot 一起修改文档。网页和桌面端支持创建、编辑；移动端首发侧重查找、阅读和分享。

**协作式 slides 和 spreadsheets 仍标为 coming soon。** 不应把“共同编辑演示文稿、完整导出”等后续能力写成已经全面上线。[Space 官方介绍](https://chatgpt.com/features/space/)

对知识整理来说，我建议每个主题只维护一张主 Page，写明更新来源、检查周期和需人工核对的字段。比如学习笔记保留“已验证结论、例子、待解问题”，项目资料保留“进度、证据、负责人、下一步”，避免每次对话都重新生成一份互相冲突的文档。

### 4. Ultrafast：用更多额度换更快生成

Astra Ultrafast 在 Codex 中的 token 生成速度最高可达 Standard 的 8 倍。这个数字衡量生成速度，不能直接当成完整任务耗时缩短 8 倍：搜索、工具执行、测试和等待外部服务仍然需要时间。

首发支持 Pro 500，以及符合条件的 Enterprise / Edu。Pro 100、Pro 200 购买额外 credits 也不能因此解锁 Ultrafast；GPT‑6.1 Sol 的 Ultrafast 仍是后续开放项目。[速度模式官方文档](https://learn.chatgpt.com/docs/agent-configuration/speed)、[6.1 Sol 发布说明](https://openai.com/index/introducing-gpt-6-1-sol/)

我的建议是：人在屏幕前反复调试、需要快速交互时再考虑加速；可以后台慢慢完成的批量工作优先用 Standard。先找出等待发生在哪里，再判断速度档位是否值得付费。

### 5. Sign in with ChatGPT：部分第三方工具能使用订阅额度

新变化包含两种独立授权：用 ChatGPT 账号登录，以及允许工具使用你的套餐额度。Plus / Pro 可在支持的工具中共享 Work / Codex 用量，不需要把 API Key 交给对方。

官方目录列出 Devin、Notion、Vercel、Warp 等额度接入伙伴，同时也单列了仅支持登录的服务。**能登录不等于能使用额度；共享额度不等于免除第三方自身订阅费。** [官方伙伴目录](https://learn.chatgpt.com/docs/sign-in-with-chatgpt)

接入后建议先在 `Settings → Usage` 为每个工具设周用量上限，观察一周再调整。这个上限是对共享总额度的限制，不是新增额度，也不会为该工具预留一份额度。额度耗尽后允许使用 credits 是另一项授权，应与自动购买 credits 的设置一起检查。[第三方套餐使用帮助](https://help.openai.com/en/articles/20001542-using-your-chatgpt-plan-in-other-apps-and-sites)

## 二、其他新功能：按工作场景看价值

| 场景 | 新内容与使用建议 |
| --- | --- |
| 开发任务跨设备运行 | **Codex Cloud** 使用可复用的云端环境。先拿一个小仓库验证依赖安装、测试和结果审阅，再扩到大型项目。[云端使用指南](https://learn.chatgpt.com/docs/cloud) |
| 终端内管理任务 | **新版 Codex CLI** 增加语音启动与引导任务、`/agents` 视图等。适合需要在终端跟踪多项工作的开发者；模型与账号权限仍需另行满足。[CLI 官方文档](https://learn.chatgpt.com/docs/codex/cli) |
| 审查代码修改 | **Code Review** 统一查看变更、评论和检查。GitHub 已正式开放，GitLab 支持仍有预览范围。要求 AI 给出代码路径与验证证据，人工决定是否接受。[代码审查文档](https://learn.chatgpt.com/docs/code-review?surface=app) |
| 持续安全检查 | **Codex Security Cloud** 扫描 GitHub 仓库并监控新提交。先校准项目威胁模型，再审阅发现与补丁；它与本地 Security 插件是不同入口。[设置文档](https://learn.chatgpt.com/docs/security/setup) |
| 在 ChatGPT 中做工具界面 | **Plugin Extensions** 支持侧边栏、对话面板、文件查看器等。Free / Go 的网页扩展仍待开放，输入框 mentions 仅桌面端支持。先实现一条完整用户流程。[扩展文档](https://developers.openai.com/plugins/build/extensions) |
| 外部事件触发工作 | **MCP Events** 让新消息、文档评论等触发自动化。先限定一个项目或频道，做好 webhook 校验和重复事件处理；当前集成不支持所有事件传输方式。[事件文档](https://developers.openai.com/plugins/build/mcp-events) |
| 团队周报与例行任务 | **Teams / Team Tasks** 支持定时或事件触发的团队工作。核对服务账号、连接权限和团队成员后再运行；团队任务消耗工作区 credits。[团队设置文档](https://learn.chatgpt.com/docs/enterprise/teams) |
| 会议转为行动项 | **Meetings 插件** 在 macOS 桌面端向 Pro / Business 提供 beta。先征得参会者同意；音频处理完成后删除，不能把它当可回放的录音档案。[Meetings 帮助](https://help.openai.com/en/articles/20001546-the-meetings-plugin-in-chatgpt) |
| 自建浏览器代理 | **Agents API 的 Computer use** 在托管浏览器中执行任务。适合网站测试、资料收集和跨页面流程，应用仍需处理访问授权并验证最终结果。[API 文档](https://developers.openai.com/api/docs/guides/agents-api/tools/computer-use) |
| 企业数据保护 | **Private Intelligence** 包含 ZDR with Private Safety Processing；Private Inference 预览计划秋季推出。企业应评估具体的数据流与保留规则。[PSP 技术文档](https://developers.openai.com/api/docs/guides/private-safety-processing) |

大会还宣布了 Decisions API 的有限预览、AWS Bedrock Managed Agents、插件创建与发现改进、Sites 承载插件、Slack / Teams 中的 ChatGPT、可分享资料页和企业 OpenAI Marketplace。它们分别面向决策路由、云平台部署、插件分发、团队工具及企业采购，普通个人用户可按需求关注。[官方发布总览](https://openai.com/index/devday-2026-recap/)

## 三、最新订阅套餐怎么比较

**这次新增的个人订阅档位是 Pro 500；Free、Go、Plus、Pro 100、Pro 200 是现有体系的比较对象。** 不同套餐还涉及产品权限，不能只按月费排名。[套餐定价](https://chatgpt.com/pricing/)、[Pro 三档说明](https://help.openai.com/en/articles/9793128-about-chatgpt-pro-tiers)

| 套餐 | 美元基准价格 | 选择时重点看什么 |
| --- | --- | --- |
| Free | $0 / 月 | 体验日常聊天；桌面端有限 Work / Codex 能力，Luna 按开放进度提供 |
| Go | 美国基准 $8 / 月 | 轻量使用，桌面端 Luna 按开放进度提供；不要按 Plus 的高级工作能力预期购买 |
| Plus | $20 / 月 | 日常研究、内容生产、个人开发的起点；支持 Work / Codex 和符合条件的第三方额度接入 |
| Pro 100 | $100 / 月 | 比 Plus 更高用量及 Pro 功能；适合持续使用代理工作的个人 |
| Pro 200 | $200 / 月 | 比 Pro 100 更多用量；已恢复新订阅，注意新旧额度过渡 |
| Pro 500 | $500 / 月 | 最高 Pro 用量，官方称为 Plus 的 25 倍；包含 Astra Ultrafast |
| Business Standard | 年付月均 $20 / 人；月付 $25 / 人 | 团队管理和业务数据保护；基准用量，至少购买 2 个付费席位 |
| Business Premium | 年付月均 $100 / 人；月付 $125 / 人 | Standard 的 5 倍用量，无 5 小时限制；dots 开放的 Business 档位 |
| Enterprise / Edu | 联系销售 | 组织治理、数据控制及合同约定；Ultrafast 等需满足额外资格 |

基础价格与 Work / Codex 范围见[官方定价文档](https://learn.chatgpt.com/docs/pricing)；Pro 500 的 25 倍口径见[大会说明](https://openai.com/index/devday-2026-recap/)。本文不沿用旧帮助页中的 Pro 100 / Pro 200 倍数；不同模型、任务和速度模式下，额度消耗不一样，最终以账号用量页为准。

Business 可以混合 Standard / Premium 席位，最低两席可以是各一席，不必全员购买 Premium；年付价格是按年结算后的月均值。Premium 新席位的价格与用量见[Business 官方说明](https://help.openai.com/en/articles/8792828-chatgpt-business-overview)和[Business FAQ](https://help.openai.com/en/articles/8542115-chatgpt-business-general-faq)。对小团队，我建议先把 Premium 分配给持续使用代理工作的成员，再按实际需求调整。

另一个实际区别是 Astra 的用量：Plus 和 Business Standard 提供有限 Astra 用量；Pro 100 / 200 与 Business Premium 可以将本身 Work / Codex 额度用于 Astra，但额度仍会消耗。Business 首发不支持 Ultrafast。[Work 与 Codex 帮助](https://help.openai.com/en/articles/20001275-chatgpt-work-and-codex)

### Pro 200 老用户尤其需要注意

最新版帮助中心已经确认：Pro 200 恢复新订阅。未满足过渡资格的新订阅使用较低的新版额度；符合资格且维持有效订阅的用户，可保留此前额度至 **2026 年 10 月 29 日**，之后转为较低额度，月费仍为 $200。

保留旧额度不会自动获得 Pro 500 或 Ultrafast。资格截止点及个人适用情况应查看账号通知与官方邮件。[最新 Pro 帮助说明](https://help.openai.com/en/articles/9793128-about-chatgpt-pro-tiers)

### 我的选购建议

- **先体验、偶尔用：Free / Go。** 先确认是否真的需要多步骤工作，再决定是否升级。
- **个人博客、学习资料、小项目开发：先试 Plus。** 用同一批真实任务观察完成率、等待时间与周用量。
- **每天长时间用 Work / Codex，或明确要 dots：考虑 Pro 100。** 同时确认所在市场和账号已开放所需功能。
- **Pro 100 持续不够用：再比较 Pro 200 与额外 credits。** 不凭旧套餐倍数购买，也不要为一次短期高峰长期升档。
- **必须用 Astra 且生成等待影响产出：评估 Pro 500。** Plus 到 Pro 500 每月多 $480；若一小时有效工作价值为 $40，需要每月节省约 12 小时才覆盖差额。这只是决策示例，应换成自己的时间价值。
- **多人协作：优先比较 Business 的具体档位。** 团队权限、数据管理和账单往往比单人额度更重要；需要 dots 时核对 Premium。

## 四、用量和账单：最容易误解的地方

### 1. Work、Codex 和第三方工具会争用额度

Work 与 Codex 共享用量；符合条件的第三方请求也计入这份预算。把额度接到更多工具，增加的是使用入口。不要把 API token 单价当成订阅额度的兑换公式。当前 Pro 没有五小时限制，仍需查看周额度及模型限制；Plus / Standard Business 的任务数量估计也不是固定消息保证。[Work / Codex 定价](https://learn.chatgpt.com/docs/pricing)、[第三方额度规则](https://help.openai.com/en/articles/20001542-using-your-chatgpt-plan-in-other-apps-and-sites)

### 2. 加速倍率与计费倍率要分开看

| 模式 | 套餐内额度消耗，相对同模型 Standard | 购买 credits / Enterprise 按量用量 |
| --- | --- | --- |
| Fast | 2.5 倍 | 2 倍 |
| Astra Ultrafast | 8 倍 | 6 倍 |

这是**消耗倍率**，并非端到端速度保证；API Key 使用单独的 API 计费规则。我的建议是默认 Standard，赶时间时才开启加速，完成后恢复。[速度与计费文档](https://learn.chatgpt.com/docs/agent-configuration/speed)

### 3. “有功能”还要核对入口和开放进度

模型在 Work 可用，不代表普通 Chat 可用；桌面支持，不代表网页或手机同时支持；企业购买套餐，也可能仍需管理员开启功能。试用前检查套餐、入口、市场与管理员设置，避免为了尚未开放的能力提前升档。

## 五、怎么把新功能用起来：三个可直接改写的任务模板

以下是建议用法，具体能否执行取决于账号能力、已连接的工具和授权范围。

### 模板 A：个人资料与内容生产

先在 Work 用 GPT‑6.1 Sol 完成一项有明确验收标准的工作，再判断是否交给 dot 持续维护：

```text
研究指定主题，只使用列出的官方来源。
输出一篇面向初学者的文章，包含事实、适用范围、使用建议和段落附近的来源链接。
先检查价格、时间、版本与可用性；无法验证的内容明确标注。
交付 Markdown 文件，并列出还需要我判断的问题。
```

验收重点是信息准确、能找到来源、文件可直接使用。试用时可以记录“人工返工了哪些地方”，这比只看文字流畅度更能判断价值。

### 模板 B：个人网站与 Vibe Coding

用 6.1 Sol 统筹一个小功能，让 Luna 承担清楚、独立的执行工作；只有困难推理需要时再使用 Astra：

```text
在现有项目中实现指定功能，先读取项目约定。
给出验收标准，只修改相关文件，保留已有成果。
可委派独立且边界明确的读取或执行任务；主代理负责集成与最终检查。
交付可运行结果、相关验证证据和未解决问题。
```

需要离线运行、保留源码或自己部署的网站，继续走本地仓库工作流；需要快速共享的工具可评估 Sites。决定前确认部署位置、可见范围与后续维护方式。[Sites 产品说明](https://chatgpt.com/features/sites/)

### 模板 C：团队周报与持续跟踪

已有 Business / Enterprise 时，可从一项低复杂度 Team Task 开始：

```text
每周五根据指定项目资料整理进展、阻塞和下周行动。
每项事实附来源，缺少证据的状态标为待确认。
只有重大变化才发送提醒；日常更新写入团队页面。
对外消息、承诺和共享系统修改按团队批准规则执行。
```

先连续运行两周，检查漏项、重复提醒、权限和费用，再考虑事件触发或更多任务。团队服务账号的权限可能与个人不同，不能只凭“我能读这个文件”判断任务访问范围。[Team Tasks 权限与计费](https://learn.chatgpt.com/docs/enterprise/teams)

## 六、我的实际采用顺序

1. **先试 6.1 Sol Standard。** 用已有真实任务比较质量与返工成本。
2. **整理资料与交付方式。** 有权限时将稳定成果放到 Pages / Space；其他情况下继续维护本地文件。
3. **接入一个真正有用的工具。** 给第三方设用量上限，观察共享额度变化。
4. **再试持续代理或团队任务。** 为 dot / Team Task 写清职责、来源、权限与通知条件。
5. **最后调整套餐和速度。** 依据每周用量、任务完成率、等待时间与人工返工决定升级。

这次大会值得带来的习惯变化，是用完整工作成果评价 AI：它是否交付了准确、可审阅、能继续维护的结果。先把一条工作流跑顺，再增加代理、连接工具或提高月费，通常更容易判断钱花在哪里。
