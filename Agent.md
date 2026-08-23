# Agent.md

本文件是 `tinywall27.github.io` 项目的文章写作与发布规范。后续 AI 参与撰写或发布文章时，必须先遵守本文件。

## 一、项目约定

- 这是一个 Jekyll 博客，文章目录是 `_posts/`。
- 首页 `/` 和 `/blog/` 会自动读取 `site.posts`。新增普通文章时，不需要修改首页、博客列表或样式文件。
- 文章页面会自动显示 Front Matter 中的 `title`，正文不要再次写同名的一级标题。
- 当前文章链接格式为 `/blog/YYYY/MM/DD/slug`。
- 项目根目录中已有用户未提交的改动时，先保留这些改动，不要擅自覆盖、删除或加入提交。

## 二、新文章文件格式

### 1. 文件位置与文件名

文章必须放在项目根目录的 `_posts/` 文件夹中，文件名必须符合：

```text
YYYY-MM-DD-english-slug.md
```

例如：

```text
_posts/2026-08-23-my-ai-note.md
```

要求：

- `YYYY`、`MM`、`DD` 分别是四位年份、两位月份和两位日期。
- 文件名中的日期应与文章发布日期一致。
- slug 优先使用英文、数字和短横线，不使用空格。
- 中文正式标题写在 `title` 中。
- 不要为了新增文章修改或重命名已有文章。

### 2. Front Matter

文件最顶部必须有 Front Matter：

```markdown
---
layout: post
title: "文章正式标题"
date: YYYY-MM-DD HH:MM:SS +0800
---
```

日期时间无法确定时，可以使用：

```yaml
date: YYYY-MM-DD
```

不要把发布日期写成未来日期，除非用户明确要求定时或预发布。

### 3. 正文排版

- Front Matter 后直接写引言，不重复写文章标题一级标题。
- 使用 `##` 作为主要段落标题，使用 `###` 作为子标题。
- 段落简洁，避免标题和正文之间出现大段空白。
- 代码使用带语言标记的代码围栏，例如标记为 `bash` 或 `javascript`。
- 路径、命令、文件名和配置项使用反引号包裹。
- 中文文章优先使用清晰、自然、短句化的表达。
- 文章涉及事实、版本、规则、价格、日期或外部资料时，必须核实；不能凭空编造来源、链接、数据或命令。
- 使用外部资料时，在相关段落附近给出来源链接；不能把推测写成事实。
- 不得写入密码、API Key、私人地址、私密文件内容或其他敏感信息。

## 三、AI 撰写文章时的流程

1. 先确认用户要写的是新文章、修改草稿，还是直接发布。
2. 先查看 `_posts/` 中的现有文章，避免标题、主题和内容重复。
3. 对用户提供的标题、路径、命令和中文原文保持准确，不擅自改写关键内容。
4. 先形成文章正文，再检查 Front Matter、文件名、链接和代码块。
5. 如果关键信息无法验证，明确标注不确定性或向用户询问，不要自行补全。
6. 用户只要求“撰写”或“保存草稿”时，不要提交或推送。
7. 只有用户明确要求“发布”“提交并发布”或同等意思时，才执行 Git 提交和推送。

## 四、发布流程

在项目根目录执行。将示例文件名和标题替换成实际内容：

```bash
git status
git add _posts/YYYY-MM-DD-english-slug.md
git diff --cached --check
git commit -m "Add post: 文章正式标题"
git push git@github.com:tinywall27/tinywall27.github.io.git master
git fetch origin master
git status --short --branch
```

发布要求：

- 不使用 `git add .`，避免把 `.DS_Store`、`draft/` 或其他无关文件加入提交。
- 提交前确认暂存区只包含本次文章及明确授权的相关修改。
- 推送优先使用上面的 SSH 地址。
- 如果推送失败，不要重复创建提交；先检查远端状态和错误原因。
- GitHub Pages 发布后，检查首页和文章详情页。构建或缓存可能需要几分钟。

## 五、发布后检查

至少检查以下内容：

- 首页 `https://tinywall27.github.io/` 是否出现新文章。
- 新文章 URL 是否符合 `/blog/YYYY/MM/DD/slug`。
- 标题是否是可点击链接。
- `git status --short --branch` 是否显示工作区干净并与 `origin/master` 同步。

如果文章没有出现，依次检查：文件名格式、Front Matter、是否推送到 `master`、GitHub Pages 部署状态，以及浏览器缓存。
