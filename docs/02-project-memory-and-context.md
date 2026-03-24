# 02｜项目记忆和上下文：为什么同一个 Claude 有时很好用，有时很离谱

Claude Code 用一阵子之后，很多人都会遇到这个问题：

- 有时候它很稳
- 有时候它很会跑偏

最常见的原因不是“模型突然变差了”，而是两件事没处理好：

1. **项目记忆没立住**
2. **当前上下文不干净**

这一章就讲这两个核心点：`/init`、`CLAUDE.md`、context。

---

## 先记住 3 句话

1. `/init` 不是装饰动作，它是在帮项目建立长期说明书
2. `CLAUDE.md` 不是生成一次就结束，它要跟着项目一起更新
3. context 不是越多越好，而是越贴当前任务越好

Claude Code 后面稳定不稳定，很多时候就看这三条有没有做好。

---

## `/init` 到底在解决什么问题

Claude Code 当然可以直接读代码库。

但只靠临时读文件，它很难稳定掌握这些东西：
- 项目怎么分层
- 哪些目录是约定
- 新组件 / hook / 页面该放哪
- 哪些事情默认不要做

这就是 `/init` 的意义。

进入项目后，直接执行：

```text
/init
```

Claude Code 会扫描项目，并生成一个 `CLAUDE.md` 文件。

通常会包含：
- 项目结构摘要
- 开发命令
- 测试命令
- 主要技术栈
- 一些架构说明

### 这里最容易犯的错

很多人执行完 `/init`，看到文件出来，就觉得结束了。

其实这时候才刚开始。

`CLAUDE.md` 更像一份草稿底座，不是最终答案。你最好马上检查：
- 结构有没有理解错
- 有没有漏掉关键约束
- 有没有把偶然实现误当成项目规范

如果不够，就自己补。

---

## `CLAUDE.md` 应该写什么

最容易理解的方式是把它看成：

> 你写给未来协作者的一页项目说明。

只不过这个协作者有时候是自己，有时候是 Claude。

一个实用的 `CLAUDE.md`，至少可以写这三类内容。

### 1）项目怎么跑

```md
## Dev commands
- install: npm install
- dev: npm run dev
- test: npm test
- lint: npm run lint
```

### 2）项目结构怎么理解

```md
## Structure
- `src/components`: reusable UI components
- `src/hooks`: reusable hooks
- `src/pages`: route-level pages
```

### 3）默认约束是什么

```md
## Conventions
- Reusable hooks go in `src/hooks`
- New pages should be route-level components only
- Do not add navigation links unless the task explicitly asks for it
```

最后一条尤其重要。

很多 AI 改代码时，最烦人的不是“完全错”，而是“顺手多做”。边界不写清楚，它就容易合理发挥。

---

## 一个很值钱的动作：把高频规则写回 `CLAUDE.md`

如果一条规则会反复影响后续生成，就别每次口头提醒，直接写进去。

比如这些都适合写回项目记忆：
- 新 hook 放哪
- 新页面要不要自动加导航
- 测试文件怎么命名
- 哪些目录默认不要碰

这样你不是在反复提醒 Claude，而是在建设项目规则层。

---

## 再说 context：为什么一个 session 里越聊越乱

`CLAUDE.md` 解决的是长期记忆。

context 解决的是：**这一次任务里，Claude 当前到底看见了什么。**

可以简单理解成：
- `CLAUDE.md`：这个项目长期是什么样
- 当前 context：我这次具体在处理什么

如果 context 太少，Claude 会猜。

如果 context 太多，它会被噪声拖偏。

这就是为什么你前半小时还在聊 basket，后半小时改按钮样式时，它还在提 basket。

不是它故意乱来，而是旧上下文还留在会话里。

---

## 我更推荐的几种 context 用法

### 场景 1：只想让它看一个文件

直接用 `@文件路径`。

```text
Please review @src/components/Button.tsx and explain how this component is structured.
```

这比“帮我看看按钮组件”强很多，因为范围非常明确。

### 场景 2：只想让它看一小段代码

如果问题只在一小段逻辑里，最好别把整文件都丢进去。直接选中片段，通常更稳。

### 场景 3：做 UI 调整

如果你是按截图或设计稿改样式，可以把图片直接拖进对话。视觉问题很多时候比文字更适合直接给图。

---

## `/clear` 和 `/compact` 怎么选

这两个命令很容易混。

### `/compact`
适合同一件事还没做完，只是历史太长了。

它不是清空，而是压缩保留。

### `/clear`
适合任务真的切换了。

比如刚才还在改路由和数据获取，现在突然开始调按钮阴影和 hover 动效，这时候最好直接清掉，甚至重开一个 session。

### 我的简单规则

- **同任务继续做**：先想 `/compact`
- **任务已经变了**：优先 `/clear` 或重开

够用了，而且很实战。

---

## 一个好例子，一个坏例子

### 坏例子

```text
Help me improve this project.
```

问题是范围太大，Claude 只能一路猜：改哪里、为什么改、什么算改好。

### 好例子

```text
Read @src/components/Button.tsx only.
Explain what this component does.
Then suggest 2 safe refactors that do not change behavior.
Do not edit anything yet.
```

这种写法的好处是：
- 文件范围明确
- 输出目标明确
- 行为边界明确
- 先分析，后动手

看起来更啰嗦，但通常更安全。

---

## 今天就能练的最小动作

### 练习 A：建立项目记忆

1. 进入项目
2. 执行 `/init`
3. 打开 `CLAUDE.md`
4. 手动补 3 条项目规则

例如：

```md
- Reusable hooks go in `src/hooks`
- Do not add nav links unless asked
- Show a plan before changing more than one file
```

### 练习 B：比较好坏 context

先试一个模糊 prompt：

```text
Please improve the button styling.
```

再试一个更清楚的：

```text
Read @src/components/Button.tsx.
Update the styling to look cleaner.
Do not change props or behavior.
Show me the plan first.
```

你大概率会明显感觉到第二种更稳。

---

## 我的判断

第 2、3 集最重要的，不是告诉你几个命令，而是让你意识到：

> Claude Code 的稳定性，不只取决于模型能力，还取决于项目规则有没有写清楚、当前上下文有没有控干净。

这两件事如果做得好，后面的 commands、MCP、subagents 才会越用越顺。
