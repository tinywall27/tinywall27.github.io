---
layout: post
title: "AI 网站开发与运维技术路线学习指南"
date: 2026-08-23
---

> **AI 驱动的个人网站**
>
> 从 Markdown、Git 与 GitHub Actions 到 Cloudflare 和 DeepSeek

这是一份面向未来 1—2 年开发与日常运维的可执行参考，适合希望用 AI 辅助开发、但仍掌握发布权与运维边界的个人站长。

> 整理日期：2026 年 8 月 17 日

## 阅读导航

这不是一份要求你一次学完全部技术的“课程大纲”，而是一张可反复查阅的地图。先理解数据怎样流动，再学习 Git 和 GitHub Actions；遇到具体问题时，再查对应工具的官方教程。

| 阅读目的 | 对应内容 |
| --- | --- |
| 先看结论 | 第 1—2 节：架构、工具边界和文件组织 |
| 准备动手 | 第 3—5 节：Git、Actions 与每日自动化流程 |
| 接入 AI | 第 6—7 节：抓取、去重、DeepSeek 与内容校验 |
| 上线运维 | 第 8—11 节：审核发布、Cloudflare、后台权限与安全 |
| 规划学习 | 第 12—14 节：成本边界、六周路线和实战任务 |
| 直接参考代码 | 附录 A—B：Workflow 与 Node.js 脚本骨架 |

> **最重要的学习优先级**
>
> Git（必须） → GitHub 基本工作流（必须） → GitHub Actions（必须） → HTTP/API 与 Node.js 脚本（够用即可） → Cloudflare 发布和 Access（按需） → 静态站点生成器 Astro（内容规模增长后再学）。

## 1. 结论：这条技术路线成立，但要加一道“人工审核闸门”

原讨论里的总体方向是合理的：静态优先、代码与内容分离、GitHub 作为核心数据源、GitHub Actions 做定时自动化、Cloudflare 做构建发布和加速、DeepSeek 做内容加工。它的最大优点是可追踪、可回滚、成本低，而且任何一层都能替换。

GitHub 不只是“网盘”，而是代码、Markdown、配置、变更历史和发布审批的唯一事实来源。

Cloudflare 不应再保存一套内容；它读取 Git 仓库的某个分支，构建出网页并分发。

AI 输出必须先成为草稿，而不是未经检查直接发布。推荐“自动开 PR → 人工审核 → 合并 main”。

自动化脚本与网页代码分目录管理。脚本可以频繁改进，但不应让 AI 每天改页面组件。

未来后台是操作入口，不是新的数据库；它最终仍通过服务端安全地修改 GitHub 中的内容。

> **推荐发布政策**
>
> 初期采用“草稿 PR”模式。等信息源稳定、提示词成熟、重复率和事实错误率经过一段时间验证后，才考虑让低风险栏目自动合并；重要文章仍保留人工审核。

## 2. 整体架构与每个工具的职责

![图 1：推荐架构——内容流与发布流围绕 GitHub 汇合]({{ '/assets/images/ai-site-architecture.png' | relative_url }})

*图 1｜推荐架构：内容流与发布流围绕 GitHub 汇合。*

| 工具 | 核心角色 | 你需要掌握到什么程度 |
| --- | --- | --- |
| Markdown | 内容载体 | 文章正文、标题、日期、标签、来源链接、审核状态。纯文本，便于 AI 生成、Git 比较和迁移。 |
| Git | 本地版本控制 | 记录每次修改，支持比较、分支、撤销和回滚；没有 Git，就很难安全地自动改内容。 |
| GitHub | 远程仓库与协作中枢 | 保存仓库、PR、Actions、Secrets、日志和权限；是“唯一事实来源”。 |
| GitHub Actions | 云端任务编排器 | 按定时或手动触发，运行 Node/Python 脚本，抓取数据、调用 AI、写文件、创建 PR。 |
| RSS / API | 机器可读信息源 | RSS 适合订阅更新；API 适合获得结构化数据。优先使用官方、稳定、有明确许可的源。 |
| DeepSeek API | 内容加工服务 | 摘要、分类、关键词、格式化；不负责部署，不直接持有 GitHub 生产权限。 |
| Cloudflare Pages/Workers | 构建、托管与边缘分发 | 监听 Git 推送、构建静态站、生成预览、发布生产站并通过全球网络分发。 |
| Cloudflare Access | 未来后台入口的身份闸门 | 在请求进入后台前校验身份；它保护入口，但不能代替后台自身的授权和服务端令牌管理。 |

## 3. 代码与内容彻底分离：仓库应该长什么样

“分离”不是必须拆成两个仓库。第一阶段用一个仓库最省心，但要在目录和数据结构上分开。只有当内容量、协作人数或权限需求明显增长时，才考虑拆成“网站仓库 + 内容仓库”。

建议的单仓库目录结构：

```text
my-site/
├─ src/                    # 页面、组件、样式：决定“怎么展示”
├─ content/
│  ├─ posts/              # 人工文章
│  ├─ news/               # 已审核资讯
│  └─ drafts/             # AI 生成、尚未发布
├─ scripts/
│  ├─ collect.mjs         # 抓取 RSS/API、清洗、去重
│  └─ summarize.mjs       # 调用 DeepSeek、输出 Markdown
├─ data/
│  ├─ sources.json        # 信息源配置
│  └─ seen.json           # 已处理条目的稳定 ID/哈希
├─ .github/workflows/
│  └─ daily-digest.yml    # 云端自动化流程
├─ public/                # 静态资源
├─ package.json
└─ README.md
```

每篇 Markdown 建议使用固定的 Frontmatter。字段越稳定，网站模板、自动化校验和未来迁移越轻松。

Markdown 内容契约示例：

```yaml
---
title: "示例标题"
date: 2026-08-17
status: draft
source_url: "https://example.com/article"
source_name: "Example"
tags: [AI, 网站运维]
summary_model: "${MODEL_NAME}"
content_hash: "sha256:..."
---

正文摘要……
```

> **数据设计原则**
>
> 保存来源 URL、抓取时间、内容哈希和模型名；不要只保存 AI 摘要。这样以后能查来源、重新生成、识别重复和比较模型输出。

## 4. Git 与 GitHub：你真正需要理解的工作流

Git 是本地版本历史；GitHub 是托管 Git 仓库并提供 PR、Actions、权限和协作界面的服务。日常最小闭环只有六个动作：查看状态、创建分支、修改、暂存、提交、推送。

| 命令/概念 | 作用 |
| --- | --- |
| `git status` | 看当前哪些文件被改动 |
| `git diff` | 看具体改了什么 |
| `git switch -c name` | 从当前状态创建并切换到新分支 |
| `git add path` | 选择要纳入本次提交的文件 |
| `git commit -m "..."` | 形成一个有说明的版本节点 |
| `git push -u origin name` | 把分支推送到 GitHub |
| Pull Request | 在 GitHub 上查看差异、讨论、检查并合并 |
| Revert | 用一个新提交撤销旧提交，保留历史；生产环境优先于强制改写历史 |

> **判断是否掌握 Git**
>
> 你能独立完成“新建分支 → 改一个 Markdown → 查看差异 → 提交 → 推送 → 开 PR → 合并 → 必要时回滚”，就足以支撑第一阶段的网站运维。

## 5. GitHub Actions：云端小助手怎样工作

一个 Workflow 是仓库里的 YAML 文件。它由触发器（`on`）、任务（`jobs`）、运行环境（`runs-on`）和步骤（`steps`）组成。GitHub 提供临时运行器执行脚本，任务结束后环境销毁，真正需要保留的结果必须写回仓库、上传 artifact 或发送到外部服务。

| 概念 | 通俗解释 |
| --- | --- |
| workflow | 一个完整自动化流程，文件位于 `.github/workflows/` |
| event / on | 触发条件，如 `schedule`、`workflow_dispatch`、`push`、`pull_request` |
| job | 一组在同一运行器上执行的步骤；多个 job 可并行或用 `needs` 串联 |
| step | 运行 shell 命令或复用 action |
| runner | 临时虚拟机，如 `ubuntu-latest` |
| secret | 敏感值，如 `DEEPSEEK_API_KEY`；只在显式引用时进入工作流 |
| variable | 非敏感配置，如模型名、抓取条数 |
| `GITHUB_TOKEN` | GitHub 为本次运行生成的仓库级临时令牌；应按最小权限配置 |

需要特别注意：

- 定时任务只在默认分支上的 Workflow 文件生效，并基于默认分支最新提交运行。
- Cron 任务可能延迟；避免把时间设在整点，并保留 `workflow_dispatch` 手动触发用于测试和补跑。
- 是否为 UTC 以及时区能力要以当前 GitHub 文档为准；无论怎样，都应在日志中打印实际时间。
- 写回仓库时显式声明 `permissions: contents: write`；默认只给读取权限更安全。
- 第三方 action 尽量选择官方/可信来源，并固定版本或完整 commit SHA，降低供应链风险。

## 6. 每日自动化的完整流程

![图 2：推荐的每日自动化流程]({{ '/assets/images/ai-daily-automation.png' | relative_url }})

*图 2｜推荐的每日自动化流程。*

| 阶段 | 具体动作 |
| --- | --- |
| 触发 | `schedule` 每天运行；`workflow_dispatch` 供手动测试。任务开始打印日期、提交号和配置摘要。 |
| 抓取 | 并发请求 RSS/API，设置超时、User-Agent、重试与失败隔离；某个源失败不必拖垮全部源。 |
| 标准化 | 统一为 `title`、`url`、`published_at`、`source`、`raw_text`、`id` 等字段。 |
| 去重 | 优先用 canonical URL / GUID；没有稳定 ID 时再对“来源+标题+日期”做哈希。 |
| 筛选 | 按发布时间、主题、语言、黑白名单和最低内容长度过滤。 |
| 调用 AI | 按条或小批次调用 DeepSeek；限制输入长度，要求严格 JSON 输出，并记录模型与用量。 |
| 校验 | 解析 JSON、检查必填字段、URL、日期、摘要长度和禁用词；失败条目放入错误报告。 |
| 生成草稿 | 写入 `content/drafts/YYYY-MM-DD/`，Frontmatter 的 `status` 保持 `draft`。 |
| 创建 PR | 仅当确有新内容时提交；PR 正文列出来源、数量、失败项和估算 token。 |
| 人工审核 | 检查事实、标题、重复、版权风险和链接；必要时编辑后再合并。 |
| 自动发布 | `main` 更新后，Cloudflare 构建并发布；其他分支用于 Preview。 |
| 观察与恢复 | Actions 与 Cloudflare 都保留日志；失败时修复后手动重跑，或 revert 合并提交。 |

## 7. RSS、API 与 DeepSeek：内容流水线的技术细节

### 7.1 RSS 与 API 的区别

RSS/Atom 是面向订阅更新的 XML 文档，通常包含标题、链接、发布时间和摘要；API 往往返回 JSON，字段更稳定、查询能力更强，但可能需要密钥、限流或付费。优先级通常是：官方 API ＞ 官方 RSS ＞授权聚合源 ＞网页抓取。网页抓取放最后，因为结构易变、版权和反爬风险更高。

| 来源 | 优点 | 主要风险 | 适合场景 |
| --- | --- | --- | --- |
| 官方 API | 结构化、稳定、字段丰富 | 密钥、限流、配额 | 数据类项目、精确字段 |
| 官方 RSS/Atom | 接入轻、适合增量更新 | 摘要可能短、XML 解析差异 | 新闻、博客、版本更新 |
| 网页抓取 | 覆盖无 API/RSS 的页面 | 易失效、合规与反爬风险 | 仅在许可明确且确有必要时 |

### 7.2 抓取脚本应有的“工程护栏”

- **网络：**超时、有限重试、指数退避、合理并发、对 429/5xx 分类处理。
- **数据：**保留原始 URL/ID/发布时间；规范化 URL；不要只靠标题去重。
- **幂等：**同一天重复运行不产生重复文件或重复 PR。
- **可观察：**日志只打印状态与计数，不打印 API Key 或完整敏感响应。
- **输入限制：**截断过长正文，避免单条内容耗尽 token；必要时先提取正文再分段摘要。
- **版权：**发布“摘要 + 来源链接 + 必要短引用”，不要自动复制整篇受版权保护的内容。

### 7.3 DeepSeek 的调用边界

DeepSeek 提供与 OpenAI SDK 兼容的聊天接口。实际项目不要把模型名散落在代码中，而应放进 GitHub Variables；API Key 放入 GitHub Secrets。模型与价格会变化，部署前以官方 [Models & Pricing](https://api-docs.deepseek.com/quick_start/pricing) 页面为准。

Node.js 调用骨架（模型名与密钥均来自环境变量）：

```js
const apiKey = process.env.DEEPSEEK_API_KEY;
const model = process.env.DEEPSEEK_MODEL;

const response = await fetch('https://api.deepseek.com/chat/completions', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    Authorization: `Bearer ${apiKey}`,
  },
  body: JSON.stringify({
    model,
    temperature: 0.2,
    messages: [
      { role: 'system', content: SYSTEM_PROMPT },
      { role: 'user', content: JSON.stringify(item) },
    ],
  }),
});

if (!response.ok) throw new Error(`DeepSeek HTTP ${response.status}`);
```

> **不要相信“能返回 JSON”就等于“JSON 一定合法”**
>
> 模型输出仍需解析、Schema 校验和错误兜底。把提示词版本、模型名和内容哈希写入元数据，未来才有可复现性。

## 8. 草稿、PR 与正式发布：三种自动化等级

| 等级 | 行为 | 优点 | 代价/风险 |
| --- | --- | --- | --- |
| A｜只生成 artifact | 任务生成文件供下载，不写仓库 | 最安全，适合第一周调试 | 每次要手工取文件 |
| B｜自动创建草稿 PR（推荐） | 机器人分支写 drafts 并开 PR | 可比较、可审核、可回滚 | 需要你每天处理 PR |
| C｜直接提交 main | 低风险栏目自动发布 | 最省操作 | 错误会直接上线；需成熟校验与监控 |

你的当前阶段建议选 B。PR 是“审核单”，不是程序员专属工具：Files changed 看 AI 实际改了哪些 Markdown；Checks 看校验是否通过；Preview deployment 看页面上线前的真实效果；确认后 Merge。

> **分支策略**
>
> `main` = 生产；`automation/YYYY-MM-DD` = 当天机器人草稿；功能开发用 `feat/xxx`。Cloudflare 生产分支指向 `main`，其他分支仅生成预览。

## 9. Cloudflare：部署、预览与缓存到底发生了什么

Cloudflare 的 Git 集成可以在 GitHub 推送后自动构建与部署。生产分支通常是 `main`；其他分支可形成 Preview。构建不是“同步数据库”，而是把仓库当前版本转换为静态 HTML/CSS/JS 并发布到边缘网络。

| 顺序 | 发生的事情 |
| --- | --- |
| 1 | GitHub `main` 收到合并提交 |
| 2 | Cloudflare 的 Git 集成收到推送事件 |
| 3 | 在干净构建环境中安装依赖并执行构建命令 |
| 4 | 把输出目录作为静态资产部署 |
| 5 | 生成部署记录和 URL；生产域名切到新版本 |
| 6 | 若构建失败，旧生产版本通常仍在；修复后重新部署 |

还需要把这些边界记清楚：

- 现有 Pages 项目可以继续使用；Cloudflare 目前对部分新项目更强调 Workers/Workers Builds，选择时以你的框架官方适配指南为准。
- 静态站本身几乎不需要数据库。只有搜索、登录、评论、在线编辑等动态能力出现时，才考虑 Workers、KV/D1/R2 或外部服务。
- 自动部署前先执行构建与链接检查；分支 Preview 用来发现布局、404 和内容格式问题。
- 构建配置（Node 版本、命令、输出目录、环境变量）应写进 README，避免只有控制台里知道。

## 10. 未来后台：Cloudflare Access 保护什么，GitHub API 又怎样接

未来的后台可以是一个只有你能访问的网页，但“Access 登录成功”不等于“浏览器可以直接拿 GitHub Token”。正确做法是让浏览器把编辑请求发给服务端函数，由服务端验证 Access 身份、校验输入，再使用受限凭据调用 GitHub API。

| 层 | 职责 | 安全边界 |
| --- | --- | --- |
| 浏览器后台 | 编辑、预览、提交发布请求 | 不保存 GitHub 长效密钥；不直接写仓库 |
| Cloudflare Access | 身份认证与访问策略 | 只允许指定邮箱/身份进入；默认拒绝 |
| Worker/服务端 API | 校验请求并调用 GitHub | 验证 Access token、做 CSRF/频率/字段校验、记录审计 |
| GitHub App（优先） | 短期安装令牌与细粒度仓库权限 | 优于把个人 PAT 暴露给前端；仅授予 contents/PR 必需权限 |
| GitHub 仓库 | 最终保存修改 | 每次后台操作对应 commit/PR，可追溯可回滚 |

> **什么时候才值得做后台**
>
> 当你已经稳定地每天发布、并且“用 GitHub 网页编辑 Markdown + 合并 PR”明显成为负担时再做。此前先把内容模型、审核流程和自动化跑稳，后台只是把成熟流程包装成更顺手的界面。

## 11. 安全、可靠性与运维检查表

### 11.1 必须做

- 把 `DEEPSEEK_API_KEY` 放在 GitHub Secrets；仓库、日志、Markdown、截图中都不出现明文。
- Workflow 顶层默认 `permissions: contents: read`；只有写草稿的 job 单独提升 `contents: write / pull-requests: write`。
- 给 Actions job 设置 `timeout-minutes`，网络请求也设置超时，避免卡死和意外计费。
- 依赖锁文件提交仓库；第三方 Actions 固定可信版本/commit；开启 Dependabot 或定期检查。
- 对 AI 输出做 Schema 校验、长度限制、URL 校验和 HTML 转义，防止坏格式和脚本注入。
- 为每日任务设置“无新增即退出”，避免空提交与无意义部署。
- 保留原始来源、哈希、处理时间、提示词版本；但不要把整篇版权内容长期写进公开仓库。
- 每月做一次恢复演练：从旧 commit 重建站点，确认回滚路径真的可用。

### 11.2 运行失败时先看哪里

| 症状 | 排查顺序 |
| --- | --- |
| Actions 没运行 | Workflow 是否在默认分支；cron/时区；Actions 是否被禁用；是否长期无活动；看 Run history。 |
| 抓取失败 | HTTP 状态码、DNS/证书、超时、429、源格式是否改变；单源失败应隔离。 |
| DeepSeek 失败 | 密钥、余额/配额、模型名、请求大小、429/5xx；重试但设置上限。 |
| 生成了重复内容 | GUID/URL 规范化、`seen.json`/索引是否被提交、脚本是否幂等。 |
| 无法 push/开 PR | `GITHUB_TOKEN` permissions、分支保护、组织策略、提交者配置。 |
| Cloudflare 不部署 | Git 集成权限、生产分支、构建命令、输出目录、Node 版本、构建日志。 |
| 上线后样式/链接坏 | 先看分支 Preview；检查 base URL、相对路径、大小写和静态资源路径。 |

## 12. 成本与配额：怎样估算而不被数字误导

这套架构的固定成本通常低，真正随使用量增长的是 AI token、动态函数请求和构建次数。具体价格与额度会变化，因此本节给出估算方法，并链接官方实时页面，不把某个短期价格当成长期承诺。

| 项目 | 计费变量 | 核对位置 | 控制方式 |
| --- | --- | --- | --- |
| 域名 | 按年 | 域名注册/续费报价 | 固定 |
| GitHub | Actions 运行分钟、存储 | 仓库 Billing / Actions 用量页 | 与是否私有仓库、运行器系统和套餐有关 |
| Cloudflare | 构建次数、文件数、Functions/Workers 请求 | Pages/Workers Limits & Pricing | 纯静态流量与动态函数不是同一计费逻辑 |
| DeepSeek | 输入 token + 输出 token | Models & Pricing 与 API usage | 压缩输入、去重、批量与缓存最有效 |
| 监控/邮件 | 告警次数或第三方服务 | 所用服务价目表 | 初期可用 Actions 失败通知与 Cloudflare 日志 |

### 估算方法

```text
月 AI 费用 ≈ 每日新增条数 × 30
          ×（平均输入 token × 输入单价 + 平均输出 token × 输出单价）/ 1,000,000

例：先在 Actions 日志记录本次条数、输入/输出 token、失败数，
连续观察 2—4 周后，再决定是否调整模型、摘要长度或抓取频率。
```

> **当前 Cloudflare Pages 官方限制提示**
>
> 截至 2026-08-17，官方 Pages 概览与 Limits 页面显示 Free 计划存在每月构建次数、单站文件数等限制；数值可能调整，正式上线前请点击本手册链接复核。

## 13. 六周学习路线：以“做出闭环”为目标

| 周次 | 主题 | 掌握点 | 验收成果 |
| --- | --- | --- | --- |
| 第 1 周 | Git 与 GitHub | 完成分支、提交、推送、PR、合并、revert；会读 `git diff` | 用 Markdown 改一篇文章并通过 PR 发布 |
| 第 2 周 | Markdown 与仓库结构 | 固定 Frontmatter；区分 src/content/scripts/data | 三篇内容能由同一模板渲染 |
| 第 3 周 | GitHub Actions | 理解 on/jobs/steps/runner/secrets/permissions；手动与定时触发 | 每天生成一个日期文件，但先不调用 AI |
| 第 4 周 | HTTP、RSS/API 与 Node.js | fetch、JSON/XML、超时、重试、去重、环境变量 | 把一个 RSS/API 转成统一 JSON |
| 第 5 周 | DeepSeek 内容流水线 | 提示词、结构化输出、Schema 校验、token 记录 | 自动生成 Markdown 草稿并开 PR |
| 第 6 周 | Cloudflare 与运维 | Preview、生产分支、构建日志、回滚、安全检查 | 审核合并后自动上线；故障可重跑/回滚 |

学习时不要先追求“把所有官方文档看完”。每周围绕一个可见成果，遇到概念再回查。你最终需要的是一条可解释、可观察、可恢复的流水线，而不是记住所有命令。

## 14. 推荐学习资源与使用顺序

### 14.1 Git / GitHub / Markdown（先学）

- [Pro Git 中文版](https://git-scm.com/book/zh/v2)｜权威免费教材；先读第 1—3 章，理解工作区、暂存区、提交和分支。
- [Git 官方 Learn 页面](https://git-scm.com/learn)｜视频、速查表和 Pro Git 入口，适合第一次建立整体概念。
- [GitHub Hello World](https://docs.github.com/en/get-started/using-github/hello-world)｜用网页完成仓库、分支、commit 和 PR，零门槛建立操作闭环。
- [GitHub Skills](https://skills.github.com/)｜互动式课程；建议从 Introduction to GitHub 开始。
- [GitHub Markdown 基础语法](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)｜写标题、链接、列表、代码块和表格的官方速查。

### 14.2 GitHub Actions（第二优先）

- [GitHub Actions Quickstart](https://docs.github.com/en/actions/get-started/quickstart)｜几分钟创建第一个 Workflow。
- [Understanding GitHub Actions](https://docs.github.com/en/actions/get-started/understand-github-actions)｜解释 workflow、event、job、step、runner 的关系。
- [Workflow syntax reference](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax)｜写 YAML 时随用随查，不建议硬背。
- [Events that trigger workflows](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows)｜重点看 schedule、workflow_dispatch、push 与 pull_request。
- [GITHUB_TOKEN authentication](https://docs.github.com/en/actions/tutorials/authenticate-with-github_token)｜理解自动化如何安全写仓库，以及最小权限。
- [Using secrets in Actions](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets)｜把 DeepSeek 密钥放入 Secrets 的官方步骤。
- [Secure use reference](https://docs.github.com/en/actions/reference/security/secure-use)｜第三方 action、令牌、日志和供应链安全。

### 14.3 抓取与 AI API（够用即可）

- [MDN：Using the Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)｜HTTP 请求与响应、错误处理的标准教程。
- [Node.js：Using Fetch](https://nodejs.org/en/learn/getting-started/fetch)｜在自动化脚本中用原生 fetch 请求网络。
- [Node.js：读取环境变量](https://nodejs.org/en/learn/command-line/how-to-read-environment-variables-from-nodejs)｜理解 process.env，避免在代码中硬编码密钥。
- [DeepSeek：Your First API Call](https://api-docs.deepseek.com/)｜官方快速开始；接口与 OpenAI SDK 兼容。
- [DeepSeek：Chat Completions](https://api-docs.deepseek.com/api/create-chat-completion/)｜请求字段、响应结构和错误处理参考。
- [DeepSeek：Models & Pricing](https://api-docs.deepseek.com/quick_start/pricing)｜每次调整模型或估算成本前先核对。

### 14.4 Cloudflare 与静态站（上线时学）

- [Cloudflare Pages：Git integration](https://developers.cloudflare.com/pages/get-started/git-integration/)｜理解 push 后自动构建、生产分支和 Git 授权。
- [Cloudflare Pages：Preview deployments](https://developers.cloudflare.com/pages/configuration/preview-deployments/)｜把 PR/分支变成上线前预览。
- [Cloudflare Pages：Limits](https://developers.cloudflare.com/pages/platform/limits/)｜上线前检查构建、文件等限制。
- [Cloudflare Access：Web applications](https://developers.cloudflare.com/cloudflare-one/access-controls/applications/http-apps/)｜未来保护后台时理解身份感知代理。
- [Cloudflare Access Learning Path](https://developers.cloudflare.com/learning-paths/clientless-access/access-application/create-access-app/)｜按步骤创建 Access 应用与策略。

### 14.5 内容规模增长后的可选框架：Astro

如果你现在已有纯 HTML 页面，不必为了“技术先进”立即重构。等文章、标签、列表页、分页和组件复用明显变多时，再考虑 Astro。它适合 Markdown 内容集合和静态输出；框架只是把内容批量编译成网页，不改变“GitHub + Actions + Cloudflare”的主架构。

- [Astro 官方 Tutorial](https://docs.astro.build/en/tutorial/0-introduction/)｜从零搭建内容型站点。
- [Astro Content Collections](https://docs.astro.build/en/guides/content-collections/)｜用 Schema 管理 Markdown 元数据，适合自动生成内容。
- [Astro 部署到 Cloudflare](https://docs.astro.build/en/guides/deploy/cloudflare/)｜核对当前推荐的 Workers/CI/CD 方式。

## 15. 最小可行实施顺序

1. 把现有网站放进 GitHub；做到 `main` 的任一提交都能被 Cloudflare 构建。
2. 建立 `content/posts` 与 `content/drafts`，先手工写三篇符合统一 Frontmatter 的 Markdown。
3. 写一个只生成测试 Markdown 的脚本；在本地多次运行，确认不会重复。
4. 用 `workflow_dispatch` 把脚本搬到 GitHub Actions；成功后再增加 `schedule`。
5. 接一个稳定 RSS/API，只做抓取、标准化和去重，不急着接 AI。
6. 加入 DeepSeek，并把 API Key 放入 Secrets；对输出做 JSON/Schema 校验。
7. 自动创建草稿分支/PR；你在 Preview 中检查后合并。
8. 运行 2—4 周，记录成功率、重复率、人工修改量和 token，再决定是否扩大信息源或提高自动化等级。

> **阶段验收标准**
>
> 你能回答“今天的内容从哪里来、脚本做了什么、AI 改了什么、谁批准发布、失败去哪里看、怎样回滚”，这套系统才算真正可运维。

## 附录 A｜GitHub Actions 工作流骨架

下面示例采用“定时 + 手动触发、最小写权限、Node.js 脚本、仅有新内容才提交”的思路。若要自动创建 PR，可在末尾改用 GitHub CLI 或可信的 PR action；上线前应固定第三方 action 的版本/commit，并结合你的分支保护规则调整。

`daily-digest.yml` 示例：

{% raw %}
```yaml
name: Daily content draft

on:
  workflow_dispatch:
  schedule:
    - cron: "17 23 * * *"   # 示例；核对 UTC/时区后再定

concurrency:
  group: daily-content
  cancel-in-progress: false

permissions:
  contents: write

jobs:
  collect:
    runs-on: ubuntu-latest
    timeout-minutes: 15
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm

      - run: npm ci

      - name: Collect and summarize
        run: node scripts/daily.mjs
        env:
          DEEPSEEK_API_KEY: ${{ secrets.DEEPSEEK_API_KEY }}
          DEEPSEEK_MODEL: ${{ vars.DEEPSEEK_MODEL }}

      - name: Validate generated content
        run: npm run validate:content

      - name: Commit only when files changed
        run: |
          if git diff --quiet; then
            echo "No new content"
            exit 0
          fi
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add content/drafts data/seen.json
          git commit -m "content: add daily drafts"
          git push
```
{% endraw %}

> **示例说明**
>
> 为了便于理解，示例直接写当前分支。正式采用 PR 模式时，应让 Workflow 创建日期分支并开 PR，或只把结果上传为 artifact。不要未经测试就把此骨架复制到生产仓库。

## 附录 B｜Node.js 内容脚本的职责分层

建议的脚本主流程（伪代码）：

```js
async function main() {
  const sources = await loadSources('data/sources.json');
  const seen = await loadSeen('data/seen.json');

  const fetched = await fetchAllWithTimeout(sources);
  const normalized = fetched.flatMap(normalizeItem);
  const newItems = dedupe(normalized, seen);

  const drafts = [];
  for (const item of newItems) {
    const result = await summarizeWithDeepSeek(item);
    const valid = validateAgainstSchema(result);
    drafts.push(toMarkdown(item, valid));
  }

  await writeDraftsAtomically(drafts);
  await updateSeenOnlyAfterSuccess(seen, newItems);
  printRunSummary({ sources, fetched, newItems, drafts });
}

main().catch((error) => {
  console.error(safeError(error));
  process.exitCode = 1;
});
```

函数分层的意义是：将来换 RSS 解析器、换模型、换网站框架或增加一个新信息源时，只替换对应模块，不推倒整条流水线。

## 附录 C｜上线前自检清单

- [ ] 本地 `npm ci + build` 成功，README 写明 Node 版本、构建命令和输出目录。
- [ ] Secrets 中有 API Key，仓库全文搜索确认没有明文密钥。
- [ ] Workflow 同时支持手动触发；cron 避开整点；设置 concurrency 与 timeout。
- [ ] 脚本连续运行两次，第二次不产生重复文件或空提交。
- [ ] AI 输出经过 Schema 校验，错误条目不会进入正式内容目录。
- [ ] 草稿 PR 可见来源 URL、摘要、标签和模型元数据。
- [ ] Cloudflare 生产分支是 `main`，PR/其他分支能生成 Preview。
- [ ] 测试一次错误构建、一次 Actions 失败、一次 revert/回滚。
- [ ] 记录当前官方限制/价格页面，设置月度用量复查习惯。

## 参考资料

以下均为本手册优先引用的官方、长期维护资料。访问日期：2026-08-17。平台界面、额度、模型和价格可能变化，请以链接中的最新内容为准。

- [1] [Git：Pro Git 中文版](https://git-scm.com/book/zh/v2)
- [2] [Git 官方 Learn 页面](https://git-scm.com/learn)
- [3] [GitHub：Hello World](https://docs.github.com/en/get-started/using-github/hello-world)
- [4] [GitHub Skills](https://skills.github.com/)
- [5] [GitHub：Markdown 基础语法](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
- [6] [GitHub Actions：Quickstart](https://docs.github.com/en/actions/get-started/quickstart)
- [7] [GitHub Actions：Understanding GitHub Actions](https://docs.github.com/en/actions/get-started/understand-github-actions)
- [8] [GitHub Actions：Workflow syntax reference](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax)
- [9] [GitHub Actions：事件与 schedule](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows)
- [10] [GitHub Actions：GITHUB_TOKEN](https://docs.github.com/en/actions/tutorials/authenticate-with-github_token)
- [11] [GitHub Actions：Secrets](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets)
- [12] [GitHub Actions：Secure use](https://docs.github.com/en/actions/reference/security/secure-use)
- [13] [MDN：Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)
- [14] [Node.js：Fetch](https://nodejs.org/en/learn/getting-started/fetch)
- [15] [Node.js：环境变量](https://nodejs.org/en/learn/command-line/how-to-read-environment-variables-from-nodejs)
- [16] [DeepSeek：API 快速开始](https://api-docs.deepseek.com/)
- [17] [DeepSeek：Chat Completions](https://api-docs.deepseek.com/api/create-chat-completion/)
- [18] [DeepSeek：Models & Pricing](https://api-docs.deepseek.com/quick_start/pricing)
- [19] [Cloudflare Pages：Git integration](https://developers.cloudflare.com/pages/get-started/git-integration/)
- [20] [Cloudflare Pages：Preview deployments](https://developers.cloudflare.com/pages/configuration/preview-deployments/)
- [21] [Cloudflare Pages：Limits](https://developers.cloudflare.com/pages/platform/limits/)
- [22] [Cloudflare Access：Web applications](https://developers.cloudflare.com/cloudflare-one/access-controls/applications/http-apps/)
- [23] [Cloudflare Access Learning Path](https://developers.cloudflare.com/learning-paths/clientless-access/access-application/create-access-app/)
- [24] [Astro：Tutorial](https://docs.astro.build/en/tutorial/0-introduction/)
- [25] [Astro：Content Collections](https://docs.astro.build/en/guides/content-collections/)
- [26] [Astro：Cloudflare 部署](https://docs.astro.build/en/guides/deploy/cloudflare/)
