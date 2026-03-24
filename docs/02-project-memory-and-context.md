# 02｜项目记忆和上下文：为什么同一个 Claude 有时很好用，有时很离谱

用 Claude Code 一段时间之后，很多人都会有一种很奇怪的感觉。

有时候它像开挂：

- 很快读懂项目
- 改动贴近现有风格
- 连目录和命名都挺顺

有时候又突然很离谱：

- 明明在改按钮，它开始扯购物车逻辑
- 明明只让它建一个 hook，它顺手改了别的文件
- 明明项目结构早就定好了，它还是像第一次进仓库一样在猜

如果只把这种波动理解成“模型今天状态不好”，你基本抓不到重点。

看完第 2、3 集之后，我觉得更准确的说法是：

> Claude Code 的表现，很大程度上取决于你有没有给它稳定的项目记忆，以及干净的当前上下文。

这一章就讲这两件事。

---

## 先给结论

如果你今天只能记住三件事，我建议记下面这三条：

1. `/init` 不是装饰动作，它是在帮你给项目建立一份长期说明书
2. `CLAUDE.md` 不是生成一次就完事的文件，它应该跟项目一起演化
3. context 不是越多越好，而是越贴近当前任务越好

很多人后面觉得 Claude Code 不稳定，根子都在这里。

不是因为它不会写，而是因为它**不知道该按什么规则写**，或者**当前会话里塞了太多不相关的历史**。

---

## 先说 `/init`：它到底在解决什么问题

Claude Code 当然可以直接读代码库。

问题是，光靠“临时读文件”，它很难稳定掌握这些东西：

- 项目怎么分层
- 哪些目录是约定俗成的
- 团队偏好的命名方式是什么
- 新组件、新 hook、新页面应该放哪
- 哪些事情默认不要做

这就是 `/init` 的作用。

### 你可以这样做

进入项目后，直接执行：

```text
/init
```

视频里给出的意思很明确：Claude Code 会扫描项目，然后生成一个 `CLAUDE.md` 文件。

这份文件通常会包含：

- 项目结构摘要
- 开发命令
- 测试命令
- 主要技术栈
- 一些架构层面的说明

如果项目本身内容比较完整，这份文件会比较有料。

如果项目还很空，它生成的内容也会比较薄。

### 这一步最容易犯的错

很多人执行完 `/init` 后，看到它生成了文件，就以为这件事结束了。

其实这时候才刚开始。

`CLAUDE.md` 更像一份**草稿底座**，不是最终答案。

你应该马上做一轮人工检查：

- 它有没有把目录结构理解错
- 它有没有漏掉关键约束
- 它有没有把一些“偶然实现”误当成“项目规范”

如果发现不够，就自己补。

---

## `CLAUDE.md` 到底该写什么

我觉得最容易理解的方法，不是把它看成“AI 配置文件”，而是把它看成：

> **你写给未来协作者的一页项目说明。**

只不过这个协作者有时候是自己，有时候是 Claude。

一个实用的 `CLAUDE.md`，至少应该覆盖这些内容：

### 1. 项目怎么跑

比如：

```md
## Dev commands
- install: npm install
- dev: npm run dev
- test: npm test
- lint: npm run lint
```

### 2. 项目结构怎么理解

比如：

```md
## Structure
- `src/components`: reusable UI components
- `src/hooks`: reusable hooks
- `src/pages`: route-level pages
```

### 3. 哪些约束是默认成立的

比如：

```md
## Conventions
- Reusable hooks go in `src/hooks`
- New pages should be route-level components only
- Do not add navigation links unless the task explicitly asks for it
```

这里最后一条很重要。视频里其实已经展示过一个典型问题：

如果你不把边界写清楚，Claude 很容易“合理发挥”，帮你顺手多做几件你没要求的事。

它未必是恶意乱改，它只是按它理解的“顺手完善”在行动。

而这恰恰是很多 AI 改代码最烦的地方：**不是完全错，而是多做。**

---

## 一个很实用的动作：把高频规则写回 `CLAUDE.md`

视频里有个细节我觉得特别值钱：

作者不是每次都重新口头说“hook 要放到 hooks 文件夹里”，而是直接把这条规则写进项目 memory。

这件事的意义很大。

因为一条经常重复的规则，如果每次都靠 prompt 说，迟早会漏。

更稳的做法是：

- 只要这条规则是项目级的
- 只要它会反复影响后续生成
- 就把它写进 `CLAUDE.md`

比如下面这些都适合沉淀进去：

- 新 hook 放哪
- 新页面需不需要自动加导航
- 测试文件命名怎么写
- 组件命名用什么风格
- 哪些目录默认不要碰

这样你就不是在“临时提醒 Claude”，而是在**建设项目规则层**。

---

## 再说 context：为什么一个 session 里越聊越乱

`CLAUDE.md` 解决的是长期记忆问题。

context 解决的是**眼前这次任务**的问题。

这两个经常被混在一起，但不是一回事。

### 一个简单理解

- `CLAUDE.md`：这个项目长期是什么样
- 当前 context：我这一次具体在处理什么

视频里对 context 的解释其实很直白：给的信息太少，Claude 会猜；给的信息太多，它会被噪声拖偏。

这就是为什么很多人会有这种体验：

- 前半小时聊的是 basket 逻辑
- 后半小时开始改按钮样式
- 结果 Claude 还在提 basket

不是它故意捣乱，而是旧上下文还留在会话里。

---

## 我现在更推荐的 context 用法

### 场景 1：只想让它看一个文件

直接在 prompt 里用 `@文件路径`。

比如：

```text
Please review @src/components/Button.tsx and explain how this component is structured.
```

这样比“帮我看看按钮组件”强很多。

因为你明确告诉它：看这个，不是看整个仓库。

### 场景 2：只想让它看一小段代码

如果问题只在一小段逻辑里，最好别把整文件都丢进去。

视频里提到，你可以直接选中片段，把局部代码作为 context。

这个动作的价值是：**减少无关信息。**

### 场景 3：你在做 UI 调整

如果你是按设计稿或者截图改 UI，可以把图片直接拖进对话。

这点很实用，因为很多样式问题用自然语言说不清，但图一给，模型立刻知道你想对齐什么。

---

## `/clear` 和 `/compact` 到底怎么选

这两个命令很容易被混着用。

我看完视频后，比较实用的记法是：

### `/compact`
适合同一件事还没做完，只是聊天历史太长了。

它的思路不是“清空”，而是“压缩保留”。

也就是说：
- 你还想沿着当前任务继续做
- 但不想背着整段冗长历史继续跑

这时候用 `/compact` 比较合适。

### `/clear`
适合任务真的切换了。

比如刚才你还在做：
- 路由改造
- 数据获取
- 表单提交

现在突然要改：
- 按钮阴影
- 卡片圆角
- hover 动效

这两件事最好别混在同一个上下文里。

这时候我更建议直接 `/clear`，甚至干脆新开一个 session。

### 我自己的简单规则

- **同任务继续做**：先想 `/compact`
- **任务已经变了**：优先 `/clear` 或重开

这个规则不花哨，但很实用。

---

## 一个好例子，一个坏例子

### 坏例子

```text
Help me improve this project.
```

这句话的问题不是不礼貌，而是几乎没有边界。

Claude 只能自己猜：
- 改哪里
- 为什么改
- 什么算改好
- 哪些地方不能碰

### 好例子

```text
Read @src/components/Button.tsx only.
Explain what this component does.
Then suggest 2 safe refactors that do not change behavior.
Do not edit anything yet.
```

这类 prompt 的好处是：
- 文件范围明确
- 输出目标明确
- 行为边界明确
- 先分析，后动手

读起来甚至有点啰嗦。

但对 Claude Code 这类会真的动项目的工具来说，**啰嗦一点通常比模糊一点更安全。**

---

## 这一章最值得马上练的动作

如果你今天准备自己练一次，我建议做这个最小实验：

### 实验 A：建立项目记忆

1. 进项目
2. 执行 `/init`
3. 打开 `CLAUDE.md`
4. 手动补 3 条项目规则

例如：

```md
- Reusable hooks go in `src/hooks`
- Do not add nav links unless asked
- Show a plan before changing more than one file
```

### 实验 B：比较好坏 context

先试一个模糊 prompt：

```text
Please improve the button styling.
```

再试一个带文件范围和边界的 prompt：

```text
Read @src/components/Button.tsx.
Update the styling to look cleaner.
Do not change props or behavior.
Show me the plan first.
```

你大概率会明显感觉到第二种更稳。

---

## 我的判断

第 2、3 集连起来之后，我最认同的一点是：

> Claude Code 的表现，不是只由模型能力决定，更是由你给它的长期规则和当前上下文共同决定。

很多人把 AI 编程的不稳定，归因成“模型今天不行”。

我现在更倾向于先问两个问题：

1. 这个项目的规则到底有没有被写清楚？
2. 这次任务的 context 到底干不干净？

这两个问题一旦处理好了，后面很多所谓“提示词技巧”反而没那么重要。

---

## 下一篇该看什么

如果你已经接受这个思路，下一步就该进入真正开始“让它动手”时的控制问题了：

- 什么权限能放
- 什么权限别乱放
- 什么时候先 planning
- 什么时候先 thinking

也就是下一章要写的内容：**permissions、planning、thinking。**
