---
title: Markdown 写作速查
date: 2026-11-02 15:24:00
categories:
  - 笔记
tags:
  - Markdown
  - 写作
description: 本站支持的常用语法，写作时对照用。
top_img: false
---

## 标题与段落

用 `#` 到 `######`，正文段落之间空一行。

## 强调

`**粗体**` → **粗体**，`*斜体*` → *斜体*，`` `行内代码` `` → `行内代码`。

## 列表

无序用 `-`，有序用 `1.`，嵌套缩进两个空格。

## 代码块

三个反引号加语言名，Butterfly 会带复制按钮和行号：

```js
function greet(name) {
  console.log(`Hello, ${name}`);
}
greet('qerhaps');
```

## 表格

| 语法 | 效果 | 备注 |
| --- | --- | --- |
| `**x**` | 粗体 | 行内 |
| `> x` | 引用 | 段落级 |
| `![alt](src)` | 图片 | 支持相对路径 |

## 引用

> 引用块用于强调观点或摘录原文。

## 图片

图片建议放 `source/img/`，引用时写 `/img/xxx.jpg`：

```markdown
![示意图](/img/demo.jpg)
```

## 文章头部字段

Butterfly 支持这些 front-matter 字段：

```yaml
---
title: 标题
date: 2026-11-02 15:24:00
updated: 2026-11-02 15:24:00
categories:
  - 笔记
tags:
  - Markdown
description: 首页摘要
cover: /img/cover.jpg      # 首页封面
top_img: false             # 关闭顶部大图
---
```
