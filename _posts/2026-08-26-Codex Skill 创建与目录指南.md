---
layout: post
title: "Codex Skill 创建与目录指南"
date: 2026-08-23
---

# Codex Skill 创建与目录指南

本文记录在 Codex 中创建 **全局 Skill** 和 **项目级 Skill** 的目录位置、终端命令及 VS Code 操作方法。

---

## 一、两种 Skill 的区别

### 1. 全局 Skill

适合希望在**所有项目中使用**的 Skill，例如：

- `teach-me`
- `explain-error`
- `review-my-code`

目录：

```text
~/.agents/skills/
```

单个 Skill 的完整结构：

```text
~/.agents/
└── skills/
    └── teach-me/
        └── SKILL.md
```

在 Mac 上，`~` 代表当前用户的 Home 目录，例如：

```text
/Users/你的用户名/.agents/skills/teach-me/SKILL.md
```

**适用场景：**

> 与具体项目无关、希望所有 Codex 项目都能调用的通用能力。

---

### 2. 项目级 Skill

适合**只服务于某个项目**的 Skill。

放在项目根目录下：

```text
项目根目录/
└── .agents/
    └── skills/
        └── skill-name/
            └── SKILL.md
```

例如：

```text
my-website/
├── .agents/
│   └── skills/
│       └── content-review/
│           └── SKILL.md
├── src/
├── package.json
└── README.md
```

**适用场景：**

> 与当前项目架构、业务规则、开发流程高度相关的 Skill。

---

# 二、创建全局 Skill

以创建 `teach-me` 为例。

## 第一步：创建目录

打开 macOS「终端」，执行：

```bash
mkdir -p ~/.agents/skills/teach-me
```

其中：

```text
mkdir       创建目录
-p          如果上级目录不存在，一并创建
~           当前用户 Home 目录
teach-me    Skill 名称
```

---

## 第二步：创建 SKILL.md

执行：

```bash
touch ~/.agents/skills/teach-me/SKILL.md
```

最终得到：

```text
~/.agents/
└── skills/
    └── teach-me/
        └── SKILL.md
```

---

## 第三步：使用 VS Code 打开

如果已经配置 `code` 命令：

```bash
code ~/.agents/skills/teach-me
```

如果想查看和管理全部全局 Skills：

```bash
code ~/.agents/skills
```

然后在 VS Code 中编辑：

```text
teach-me/SKILL.md
```

---

# 三、如果终端不能使用 `code`

如果执行：

```bash
code ~/.agents/skills
```

出现：

```text
command not found: code
```

说明 VS Code 的 `code` 命令尚未加入 PATH。

打开 VS Code，按：

```text
⇧ Shift + ⌘ Command + P
```

打开命令面板。

搜索：

```text
Shell Command: Install 'code' command in PATH
```

执行后重新打开终端。

然后再运行：

```bash
code ~/.agents/skills
```

---

# 四、不使用终端，在 VS Code 中打开全局 Skills

打开 VS Code：

```text
文件
↓
打开文件夹…
↓
按 ⇧⌘G
↓
输入 ~/.agents/skills
↓
回车
↓
打开
```

由于 `.agents` 是以 `.` 开头的隐藏目录，直接浏览时可能看不到。

使用：

```text
⇧⌘G
```

直接输入路径最方便。

---

# 五、通过 Finder 打开

在 Finder 中按：

```text
⇧⌘G
```

输入：

```text
~/.agents/skills
```

回车即可进入隐藏的 Skills 目录。

也可以随后把该文件夹拖入 VS Code。

---

# 六、创建项目级 Skill

假设当前项目为：

```text
my-website/
```

先进入项目：

```bash
cd /你的项目路径/my-website
```

然后创建 Skill：

```bash
mkdir -p .agents/skills/content-review
```

创建 `SKILL.md`：

```bash
touch .agents/skills/content-review/SKILL.md
```

最终：

```text
my-website/
├── .agents/
│   └── skills/
│       └── content-review/
│           └── SKILL.md
├── src/
├── package.json
└── ...
```

如果当前终端已经位于项目根目录，可以直接：

```bash
code .agents/skills/content-review
```

或者查看该项目全部 Skills：

```bash
code .agents/skills
```

---

# 七、最常用命令速查

## 创建全局 Skill

```bash
mkdir -p ~/.agents/skills/teach-me
touch ~/.agents/skills/teach-me/SKILL.md
code ~/.agents/skills/teach-me
```

对应：

```text
~/.agents/skills/teach-me/SKILL.md
```

---

## 创建项目级 Skill

先进入项目根目录，然后：

```bash
mkdir -p .agents/skills/my-skill
touch .agents/skills/my-skill/SKILL.md
code .agents/skills/my-skill
```

对应：

```text
项目根目录/.agents/skills/my-skill/SKILL.md
```

---

# 八、如何选择

简单记住：

```text
这个 Skill 是否希望其他项目也使用？
            │
       ┌────┴────┐
       │         │
      是         否
       │         │
       ▼         ▼
全局 Skill     项目级 Skill
       │         │
       ▼         ▼
~/.agents/    项目/.agents/
skills/       skills/
```

例如：

```text
teach-me
→ 所有项目都可能需要
→ 全局 Skill

explain-error
→ 所有项目都可能需要
→ 全局 Skill

某个网站专属内容审核规则
→ 只针对该网站
→ 项目级 Skill

某个项目专属部署流程
→ 只针对该仓库
→ 项目级 Skill
```

---

## 最重要的两个路径

**所有项目可用：**

```text
~/.agents/skills/<skill-name>/SKILL.md
```

**当前项目使用：**

```text
<project-root>/.agents/skills/<skill-name>/SKILL.md
```

对于 `teach-me` 这种通用 Skill，推荐：

```text
~/.agents/skills/teach-me/SKILL.md
```
