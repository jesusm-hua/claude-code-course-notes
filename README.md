# Claude Code 学习笔记

一份基于 **Net Ninja《Claude Code Tutorial》系列视频**整理的中文学习笔记。

这份仓库不追求“把所有功能都讲全”，重点是把 Claude Code 讲清楚：
- 它适合怎么用
- 新手应该先学什么
- 哪些能力值得先上手
- 哪些地方最容易误用

## 基于什么整理

主要基于以下材料：

- Net Ninja《Claude Code Tutorial》正式教程 10 集
- 播放列表中的补充短片 1 条（只作路线参考，不作为主体内容）
- 视频 metadata
- 自动字幕
- 整理过程中的结构化笔记与复盘

说明：
- 本仓库不是官方文档翻译
- 不是逐字转录
- 个别命令名、快捷键、模型名若字幕不稳定，以 Claude Code 当前版本文档和 CLI 实际行为为准

## 谁写的

- 整理与写作：**HuaClaw**
- 面向对象：希望把 Claude Code 真正放进开发工作流的开发者

## 这份仓库适合谁

适合这几类读者：

- 刚接触 Claude Code，想快速建立正确使用姿势
- 已经开始用，但效果忽好忽坏
- 想理解 `CLAUDE.md`、context、slash commands、MCP、subagents、GitHub integration 这些能力应该怎么串起来
- 不想先看一堆抽象概念，想先抓住能直接上手的东西

## 推荐阅读顺序

建议按这个顺序看：

1. [01 - 先跑起来：Claude Code 到底该怎么开始](docs/01-getting-started.md)
2. [02 - 项目记忆和上下文：为什么同一个 Claude 有时很好用，有时很离谱](docs/02-project-memory-and-context.md)
3. 后续再看权限、planning / thinking、commands、MCP、subagents、GitHub

原因很简单：多数人第一次用崩，不是因为不会高级功能，而是因为：
- 项目记忆没立住
- 上下文没控住

## 仓库结构

### 成品教程
- `docs/01-getting-started.md`
- `docs/02-project-memory-and-context.md`
- 后续章节会继续补齐

### 原始学习笔记
- `notes/episodes-1-3.md`
- `notes/episodes-4-7.md`
- `notes/episodes-8-10.md`

### 旧版汇总文档
先保留，后续会逐步拆到新结构里：
- `knowledge-doc.md`
- `learning-route.md`
- `episode-notes.md`

## 一句话方法

> Claude Code 最成熟的用法，不是把开发交给 AI，而是把它放进一个有项目记忆、有上下文边界、有人类复核的开发流程里。

## 你现在就可以试的第一步

找一个你不怕折腾的项目，进入项目根目录后：

```bash
claude
```

然后做三件事：

1. 先让它总结这个项目在干什么
2. 再执行 `/init`
3. 看看它生成的 `CLAUDE.md` 写了什么

如果你想直接开始，先看第一篇：

**[01 - 先跑起来：Claude Code 到底该怎么开始](docs/01-getting-started.md)**
