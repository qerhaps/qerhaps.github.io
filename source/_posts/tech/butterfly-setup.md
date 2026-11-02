---
title: 用 Butterfly 主题重搭这个博客
date: 2026-11-02 15:20:00
updated: 2026-11-02 15:20:00
categories:
  - 技术
tags:
  - Hexo
  - Butterfly
description: 记录从 landscape 切到 Butterfly 5.7 的完整过程，包括依赖、配置分离和部署分支的取舍。
top_img: false
---

## 为什么换主题

`landscape` 够用，但卡片布局、本地搜索、夜间模式这些都没有。Butterfly 是 Hexo 生态里维护得比较勤的一款，配置项多但没有强绑定，换起来成本可控。

## 换主题的三步

### 1. 拉到 themes 目录

```bash
git clone -b master --depth 1 \
  https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

拉完记得删掉 `themes/butterfly/.git`，否则主题会被当成子模块，提交时只留一个空引用。

### 2. 补渲染器

Butterfly 的模板是 pug，样式是 stylus，两个渲染器缺一不可：

```bash
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

### 3. 配置分离

主题自带的 `_config.yml` 不要直接改，复制到站点根目录重命名为 `_config.butterfly.yml`。Hexo 会优先读根目录这份，主题升级时不会被覆盖。

```bash
cp themes/butterfly/_config.yml _config.butterfly.yml
```

最后把站点 `_config.yml` 里的 `theme` 改成 `butterfly`。

## 踩到的两个坑

| 现象 | 原因 | 处理 |
| --- | --- | --- |
| 主题样式全丢 | 缺 `hexo-renderer-stylus` | 补装渲染器后 `hexo clean` |
| 字数统计不显示 | `wordcount` 默认关闭 | 装 `hexo-wordcount` 并置 `enable: true` |

## 部署分支的选择

仓库名是 `qerhaps.github.io`，Pages 的可选源有限。最后采用 **main 存源码 + gh-pages 存产物**：

```yaml
deploy:
  type: git
  repo: https://github.com/qerhaps/qerhaps.github.io.git
  branch: gh-pages
```

好处是源码干净可回溯，产物不进入提交历史；代价是 Pages 设置里要手动把源指到 `gh-pages`。
