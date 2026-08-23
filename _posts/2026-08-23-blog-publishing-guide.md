---
layout: post
title: "博客文章发布流程与格式要求"
date: 2026-08-23 18:15:00 +0800
---

这篇文章记录本博客发布新文章时需要遵守的文件格式、目录位置和 Git 发布流程。

## 一、文章放在哪里

所有博客文章都放在项目根目录的 `_posts` 文件夹中：

```text
tinywall27.github.io/
└── _posts/
    ├── 2026-08-23-codex-skill-creation-guide.md
    └── 2026-08-23-blog-publishing-guide.md
```

文章列表会自动读取 `_posts` 中符合规则的文章，不需要另外修改首页或博客列表模板。

## 二、文件名格式

文件名必须使用下面的格式：

```text
YYYY-MM-DD-文章短名称.md
```

例如：

```text
_posts/2026-08-23-my-ai-note.md
```

建议文件名使用英文、数字和短横线，中文标题写在文章的 `title` 中。文件名前面的日期必须是四位年份、两位月份和两位日期。

## 三、文章开头的 Front Matter

每篇文章都必须在最顶部写入 Front Matter，并用两组 `---` 包住：

```markdown
---
layout: post
title: "我的 AI 学习笔记"
date: 2026-08-23
---
```

字段说明：

- `layout: post`：使用博客文章页面的版式。
- `title`：文章在列表和文章页中显示的标题。
- `date`：文章发布日期，也会用于文章排序和生成链接。

当前网站的文章链接格式是：

```text
/blog/年/月/日/文件名中的短名称
```

例如，`2026-08-23-my-ai-note.md` 会生成：

```text
/blog/2026/08/23/my-ai-note
```

## 四、正文写法

Front Matter 结束后，直接写 Markdown 正文：

````markdown
这是一段正文。

## 二级标题

这里继续写内容。

### 三级标题

- 列表项目一
- 列表项目二

```bash
echo "代码示例"
```
````

文章页面已经自动显示文章标题，所以正文开头不需要再写一遍同样的一级标题。正文可以直接从引言或 `##` 小标题开始。

## 五、发布到网站

在项目根目录执行：

```bash
git status
git add _posts/2026-08-23-my-ai-note.md
git commit -m "Add post: 我的 AI 学习笔记"
git push git@github.com:tinywall27/tinywall27.github.io.git master
```

其中，`git add` 后面的文件名要替换成实际新增的文章文件名。

推送成功后，GitHub Pages 会自动构建网站。首页和 `/blog/` 页面会自动显示新文章，不需要手动编辑文章列表。

## 六、发布前检查清单

- 文件位于 `_posts` 文件夹。
- 文件名以 `YYYY-MM-DD-` 开头，并以 `.md` 结尾。
- Front Matter 位于文件最顶部。
- `layout` 写成 `post`。
- `title` 使用中文或希望展示的正式标题。
- 正文中不重复写文章一级标题。
- 已经执行 `git add`、`git commit` 和 `git push`。
- 推送后刷新 `https://tinywall27.github.io/` 检查文章列表。

如果文章没有出现，优先检查文件名、Front Matter 和推送分支。GitHub Pages 构建通常需要等待几分钟。
