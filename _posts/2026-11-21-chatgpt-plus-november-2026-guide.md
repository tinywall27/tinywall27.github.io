---
layout: post
title: "ChatGPT Plus 11 月更新实用指南｜Space、GPT‑6.2 与日常工作流"
date: 2026-11-21
---

这轮更新里，Plus 用户最值得试的是 **Space 和 GPT‑6.2**。Space 可以把对话里的研究、课件和待办整理为长期编辑的页面。GPT‑6.2 则给 Work 和 Codex 增加了两种选择，日常任务先用 Sol，跨材料研究或复杂代码改动再考虑 Astra。再加上实时听写和能做 GIF 的 Images 2.6，从口述想法到交付一份可修改的成果，可以少做几次复制粘贴。

本文核验至 **2026 年 11 月 21 日（北京时间）**。表中日期沿用 OpenAI 公告。有些功能会分账号推送，下面的操作示例可以直接复制试用。[ChatGPT 官方更新日志](https://help.openai.com/en/articles/6825453-chatgpt-release-notes)

## 哪些更新与 Plus 有关

| 公告日期 | 功能 | Plus 可用情况与入口 |
| --- | --- | --- |
| 11 月 20 日 | **Space 与协作式 Slides** | 向年满 18 岁的 Plus 用户分批开放。在网页或桌面版侧边栏找 **Space**，也可以在 Work 里直接请 ChatGPT 创建页面或演示文稿。[Space 指南](https://help.openai.com/en/articles/20001549-getting-started-with-space-in-chatgpt) · [Slides 指南](https://help.openai.com/en/articles/20001605-creating-and-collaborating-on-slides-in-chatgpt) |
| 11 月 17 日 | **GPT‑6.2 Astra、Sol** | Plus 可在 **Work** 或 **Codex** 选择，处于预览阶段。日常 Chat 使用自己的模型选项。[模型上线说明](https://learn.chatgpt.com/docs/whats-new/gpt-62-astra-sol) |
| 11 月 18 日 | **实时听写** | 面向全部套餐逐步推出，Plus 也能用。在消息输入框点麦克风，边说边看转写。[更新日志](https://help.openai.com/en/articles/6825453-chatgpt-release-notes) · [听写说明](https://help.openai.com/en/articles/12168547-voice-dictation-faq) |
| 11 月 4 日 | **Images 2.6** | 基础图像功能覆盖所有套餐，GIF 正分批推出，Plus 也支持带推理的图像生成。可从 **Images** 或普通对话开始。[图像功能指南](https://help.openai.com/en/articles/11084440-images-in-chatgpt) |

11 月 19 日公布的手机新版界面主要面向满足条件的 Pro 用户，只有少量符合条件的 Plus 账号会先看到。个人版 Dot 仍属于 Pro 权益。11 月 18 日发布的 Accounting 插件要以插件目录是否出现 **Install plugin** 为准，官方并未承诺每个 Plus 账号都能安装。[更新日志](https://help.openai.com/en/articles/6825453-chatgpt-release-notes) · [Accounting 安装说明](https://help.openai.com/en/articles/20001709-using-chatgpt-for-accounting-work)

## 用 Space 把研究和课件留在同一个地方

账号开放后，侧边栏的 **Space** 会接替 Library，原来放在 Library 的文件可以到 Space 里找，已有的 **Projects** 仍用于管理项目对话和指令。第一次可以在网页端点 **Space → New page**，也可以打开已有 Work 对话，直接说“把这段讨论整理成一份 Space 页面”。页面可以手动改，也可以选中文字请 ChatGPT 改。团队审阅时，可以在评论里 `@ChatGPT`，把修改指令落到具体段落。[Space 入门](https://help.openai.com/en/articles/20001549-getting-started-with-space-in-chatgpt) · [页面操作](https://learn.chatgpt.com/docs/space/pages) · [在页面中使用 ChatGPT](https://learn.chatgpt.com/docs/space/agents)

例如，围绕《只知道纬度，就能判断一个地方的气候吗？》做一个备课页面。以后换城市、补气候数据、改探究任务，都可以在原页面接着做。把已核验的资料和待核项一并保存在页面里，过几周再打开也能看懂当初为什么选这组案例。

```text
请在 Space 创建「七月避暑攻略｜世界气候课例」页面。
用我提供的气候资料设计学习任务 1、2、3，让初中生先判断，再用证据修正判断。
选一组纬度接近、七月气候差异明显的城市。把城市、纬度、海拔、七月温度、
数据统计时段和原始链接汇成表格。待核实的数据留空并说明原因。
把任务说明、预期学生回答、教师追问和证据表放在同一页。
```

需要做课件时，在 Work 里说“根据这份页面创建 6 页 **ChatGPT Slides**”，也可在 Space 选 **Create → Slide**。网页版可以导出 PowerPoint、PDF 或 Google Slides。Windows 10 上想用协作式 Slides，直接走网页版或云端模式。课堂上如果学生需要直接打开，优先用导出的 PDF 或学校已有的课程平台，因为 Space 暂无公开链接，受邀者也得登录具备 Space 权限的账号。[Slides 创建、共享与导出](https://help.openai.com/en/articles/20001605-creating-and-collaborating-on-slides-in-chatgpt)

### 让一页周报自己更新

OpenAI 已宣布页面和幻灯片可借助已连接的应用或工具定期更新。当前操作文档建议**在对话里请 ChatGPT 创建定时任务**，再检查已保存的安排和第一次运行结果。发布初期，页面里的 **Keep Updated** 按钮尚未开放。用 **Scheduled** 核对运行时间和结果，有同用途的任务就直接改那一项。[Space 定期更新说明](https://learn.chatgpt.com/docs/space/agents) · [定时任务指南](https://help.openai.com/en/articles/10291617-scheduled-tasks-in-chatgpt)

```text
每周一上午 9 点（北京时间）检查 OpenAI、GitHub 和 Cloudflare 的官方更新。
关注会影响个人站点的 Codex 额度、定价、兼容性，以及 GitHub Actions、Cloudflare Pages 的流程变动。
有重大变化时，在「个人站点维护」页面追加一条，写明日期、官方来源、影响和应对办法，并提醒我。
若已有同用途的定时任务，更新原任务。完成设置后确认任务名称、时区和下次执行时间。
```

如需给同事分享，先核对页面里的附件和摘录。直接上传到页面的附件会跟随页面权限，外部文件链接则继续使用原服务的权限。把原文复制到页面后，页面的读者就能看到那部分内容。[Space 文件与共享权限](https://help.openai.com/en/articles/20001549-getting-started-with-space-in-chatgpt)

## GPT‑6.2 怎么选，额度怎么用

**Work** 更适合从资料出发，研究、写稿、做表格或演示。**Codex** 更适合从项目仓库出发，改文件、跑检查、看差异。进入 Work 的模型选择器，如果界面显示 **Power** 滑块，就打开 **Advanced** 选模型和推理力度。Codex 桌面版用模型选择器，CLI 用 `/model`。OpenAI 在 11 月 20 日发布的 CLI `0.171.0` 中已加入两个模型。[GPT‑6.2 使用说明](https://learn.chatgpt.com/docs/whats-new/gpt-62-astra-sol) · [Codex CLI 更新日志](https://learn.chatgpt.com/docs/changelog)

| 任务 | 建议起点 | 怎么验收 |
| --- | --- | --- |
| 写博客、根据反馈修改课件、给站点新增一篇文章 | **Sol** | 核对来源、排版和输出文件，检查 Git diff 是否只涉及本次文章 |
| 综合多份课标和论文，核对教学案例的论证链 | **Astra** | 要求逐条给出证据出处、适用范围和待核实之处 |
| 排查跨主题模板、构建和部署的站点故障 | 先用 **Sol** 收集日志和定位范围，跨多个环节仍难定位时用 **Astra** | 检查原因能否由日志复现，改动后跑相关检查并验证上线结果 |

Plus 的 **Work 和 Codex 共用额度**。OpenAI 按本地任务给出的每 5 小时估算，Sol 约 **17–180 条**，Astra 约 **6–50 条**。具体消耗取决于任务复杂度、上下文、工具调用和运行位置，也可能有周限额。查看用量面板，以账号显示的用量和重置时间为准。日常可以用 Sol 完成整理与初稿，把 Astra 留给一轮有明确问题和验收目标的复核。[官方用量与计费说明](https://learn.chatgpt.com/docs/pricing)

### 基金月结可以先从 Excel 副本试起

如果 Accounting 插件还装不了，可先用 Plus 已支持的 **ChatGPT for Excel**。在 Excel 的 **主页 → 加载项** 搜索 ChatGPT，安装后在功能区打开并登录。要用桌面版 Codex 直接检查已打开的工作簿，先让加载项和 Codex 登录同一账号与工作区，保持工作簿及加载项打开，再在 Codex 里提及 `@Microsoft Excel`。找不到工作簿时，可在加载项的 `•••` 菜单检查 **Allow Codex control** 是否开启。[Excel 与 Codex 官方操作指南](https://help.openai.com/en/articles/20001063-chatgpt-for-excel-and-google-sheets)

```text
请检查当前打开的脱敏月结样表副本，保留原有工作表和公式。
在新工作表列出无法匹配的交易、疑似重复项和金额差异，逐项附上来源表、单元格和计算依据。
按币种给出差异小计，汇率口径不明的标为待确认，由我复核后再处理。
```

第一轮适合用虚构或已获许可的脱敏数据，重点看异常能否追到原单元格、不同币种是否误加总、原公式是否被改动。结果经人工核对后，再决定哪些步骤可以纳入月结模板。[Excel 数据与结果复核说明](https://help.openai.com/en/articles/20001063-chatgpt-for-excel-and-google-sheets)

## 实时听写和 GIF 适合哪些小任务

**听写适合先把想法说完整。** 点输入框的麦克风开始说话，点击波形可以显示或隐藏转写文本。发送前可补标点、改专有名词。新版移除了原先 10 分钟的录音限制。比如刚听完一节课，可以口述学生在哪一步改了判断，再请 Work 整理成听课记录。给视频时间线记素材时，也可以顺口说出“原声保留”“横竖屏”“哪段放片头”，最后整理成镜头清单。[实时听写更新](https://help.openai.com/en/articles/6825453-chatgpt-release-notes) · [听写使用说明](https://help.openai.com/en/articles/12168547-voice-dictation-faq)

**Images 2.6 适合制作能反复使用的封面和插图。** 上传自己有权使用的图片，请 ChatGPT 保存为具名参考图，之后在新对话用 `@名称` 调用。要动图，就明确说“做一个循环 GIF”，并指定对象哪里动、哪里保持静止。例如“让纸飞机缓慢掠过，背景和标题保持静止”。打开成品可以调播放速度和保存。它很适合博客配图或视频片头的点缀，用于教学时，精确地图和气候图表应从真实数据绘制。[Images 2.6 发布说明](https://openai.com/index/introducing-images-2-6/) · [参考图与 GIF 操作](https://help.openai.com/en/articles/11084440-images-in-chatgpt)

## 第一次用，先找一项手头的任务

要备课，就把正在写的课例做成一页 Space，再从同一份材料生成课件。要维护网站，就在 Codex 用 Sol 做一次带检查的改动。要处理月结，就用一份样表试做差异清单。**用已有任务试一次，最容易判断这项功能能否省掉你原来手动完成的一步。**
