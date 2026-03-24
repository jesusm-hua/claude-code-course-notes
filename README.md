# Claude Code 学习笔记

> 基于 Net Ninja《Claude Code Tutorial》系列视频整理。
> 
> 这不是官方文档翻译，也不是“看完视频后的大总结”。
> 我更想把它写成一套**技术学习笔记**：讲清楚 Claude Code 到底适合怎么用、先学什么、哪里容易误用，以及哪些东西值得马上上手试一遍。

## 这份仓库适合谁

适合这几类人：

- 刚接触 Claude Code，想快速建立正确使用姿势
- 已经会装、会跑，但总觉得效果忽好忽坏
- 想把 Claude Code 真正放进自己的开发工作流，而不是只把它当聊天工具
- 对 `CLAUDE.md`、context、slash commands、MCP、subagents、GitHub integration 这些概念有兴趣，但不想一上来就看一堆抽象总结

如果你更想看的是“官方参数字典”或者“完整命令大全”，这份笔记不是那个方向。这里更偏：

- 先讲问题
- 再给动作
- 再解释原理
- 顺手补踩坑和我的判断

## 我为什么要重写这套笔记

第一版写完之后，我自己回看就有点不满意：道理是对的，但太像“归纳报告”，不太像真正在用的人写出来的学习记录。

Claude Code 这类工具，如果只写成抽象原则，很容易出现一种假懂：

- 知道它强调 context
- 知道它有 `CLAUDE.md`
- 知道它能接 MCP
- 知道它能做 subagents

可真到自己动手的时候，还是会卡在这些问题上：

- 第一次进项目，我到底先干什么？
- `/init` 生成完以后，我要不要改？改什么？
- 为什么一个 session 里越聊越乱？
- 什么时候该让它 plan，什么时候该让它 think？
- slash command 值不值得写？
- GitHub 自动化是不是一上来就该接？

所以我把第二版改成了现在这个方向：**少一点概括，多一点可执行动作。**

## 推荐阅读顺序

如果你是第一次看，建议别从“最强功能”开始，按下面这个顺序更顺手：

1. [01 - 先跑起来：Claude Code 到底该怎么开始](docs/01-getting-started.md)
2. [02 - 项目记忆和上下文：为什么同一个 Claude 有时很好用，有时很离谱](docs/02-project-memory-and-context.md)
3. 后续再看：权限、planning / thinking、commands、MCP、subagents、GitHub

我为什么建议这样读？因为 Claude Code 最容易误导人的地方，就是让你觉得“先学高级功能更值钱”。

其实不是。

多数人第一次用崩，通常不是因为不会 MCP，也不是因为不会 subagent，而是因为前两步没打好：

- 项目记忆没立住
- 上下文没控住

这两个问题不解决，后面接再多能力，通常只会更快跑偏。

## 这份仓库怎么组织

### 成品教程
- `docs/01-getting-started.md`
- `docs/02-project-memory-and-context.md`
- 后续章节会继续补齐

### 原始学习笔记
- `notes/episodes-1-3.md`
- `notes/episodes-4-7.md`
- `notes/episodes-8-10.md`

### 旧版汇总文档
这些先保留，后续会逐步拆进新结构：
- `knowledge-doc.md`
- `learning-route.md`
- `episode-notes.md`

## 先记住一句话

如果你今天只带走一句话，我希望是这句：

> Claude Code 最成熟的用法，不是“把开发交给 AI”，而是把它放进一个有项目记忆、有上下文边界、有人类复核的开发流程里。

## 你现在就可以做的第一件事

找一个你不怕折腾的项目，进项目根目录，启动 Claude Code，然后先别急着让它写功能。

先做这三步：

```bash
claude
```

进入会话后：

1. 先让它总结这个项目在干什么
2. 再执行 `/init`
3. 看看它生成的 `CLAUDE.md` 写了什么

只要你认真跑完这一轮，后面很多内容都会更容易理解。

---

如果你想直接开始，下一篇看这里：

**[01 - 先跑起来：Claude Code 到底该怎么开始](docs/01-getting-started.md)**
