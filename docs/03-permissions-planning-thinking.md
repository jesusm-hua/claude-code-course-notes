# 03｜权限、Planning、Thinking：Claude Code 真开始动手时，怎么控住它

前两章解决的是两件事：
- 先把 Claude Code 放进项目里
- 再把项目记忆和上下文立住

接下来就进入真正容易翻车的地方了：**让它开始改代码。**

这时候有三个问题一定要分清：

1. 哪些操作应该放权，哪些不该
2. 什么任务该先 planning
3. 什么任务该先 thinking

如果这三个问题混在一起，Claude Code 很容易从“好帮手”变成“高效率制造返工”。

---

## 先记住 4 句话

1. `read`、`edit`、`bash` 不是一个风险等级
2. **Planning 管范围，Thinking 管复杂度**
3. plan 不是批准书，它只是第一轮方案
4. 任务越大，越要拆；权限越高，越要审

---

## 先说权限：别一上来就求快

Claude Code 会自动选工具，但你得自己知道不同工具的风险差别。

最常见的三类可以这样理解：

### `read`
风险最低。

它主要是在读文件、读项目结构、读现有内容。多数情况下，这类操作可以更宽松。

### `edit`
中风险。

一旦开始写文件，哪怕只是小改动，也可能改到你没注意的地方。尤其是多文件任务，Claude 往往不是一步完成，而是连续几步编辑。

### `bash`
高风险。

因为命令的副作用不一定只停在“看一眼”。它可能：
- 改文件
- 跑脚本
- 写缓存
- 触发构建
- 动 Git 状态

所以对 bash 类动作，我更建议保守一点。

---

## 一个简单权限原则

如果你不想记太多规则，先记这个就够了：

- **读**：相对宽松
- **写**：保留审查
- **执行命令**：按白名单放权，不要泛放

视频里提到一个很实用的点：有些高频、低风险命令可以做项目级允许列表。比如：
- 测试命令
- lint 命令
- 某些只读 Git 命令

这样能减少重复确认。

但别因为嫌麻烦，就把一大类 bash 权限全开。很多问题就是从这里开始的。

---

## Planning 到底适合什么

我觉得最容易理解的说法是：

> **Planning 适合那种“逻辑不一定最难，但改动范围已经明显变大”的任务。**

比如：
- 一个组件要拆成多个子组件
- 一个页面要替换掉现有实现
- 一次改动会动到 3 个以上文件
- 你已经知道要做什么，但想先确认实施路径

这时候让 Claude 直接开改，风险往往不在单点代码，而在于它会怎么铺开范围。

### 一个典型 prompt

```text
Before editing anything, make a plan for this task.
List the files you expect to change and why.
Do not implement yet.
```

这类 prompt 很有价值，因为它先把“动作”变成“计划文本”。

而计划文本本身就是第一轮 review 材料。

---

## Thinking 适合什么

Thinking 不是更高级的 planning。

它更适合这种任务：
- 范围不一定特别大
- 但逻辑链很复杂
- 需要先推演方案，不能一边想一边乱写

比如：
- 带认证的评论系统
- 多角色权限判断
- 带状态机的交互逻辑
- 涉及数据一致性的流程

这类任务的难点不是“改几个文件”，而是“你到底先把逻辑想清楚没有”。

### 一个典型 prompt

```text
Think through the implementation first.
List the edge cases, risks, and trade-offs.
Do not edit code until the approach is clear.
```

如果你已经知道这是复杂逻辑题，那就别让它一边猜一边写。

---

## Planning 和 Thinking 最容易混的地方

很多人会把它们都理解成：

> 先让 Claude 聪明一点。

这不够准确。

更实用的区分是：

### 该 planning 的任务
重点在：
- 会影响哪些文件
- 顺序怎么走
- 改动边界在哪

### 该 thinking 的任务
重点在：
- 逻辑有没有想清楚
- 边界条件是什么
- 有没有遗漏反例和异常流程

一句话：

- **改得广** → 先 planning
- **想得深** → 先 thinking

---

## 一个好用的组合拳

真实任务里，这两个也可以组合。

比如做一个评论系统：

1. 先 thinking：想清楚认证、审核、数据流、异常处理
2. 再 planning：列出要改哪些文件、先后顺序怎么走
3. 最后才开始动代码

这种顺序通常比“先写起来再说”稳很多。

---

## 一个坏例子

```text
Build the whole comments feature for this app.
```

这类 prompt 最大的问题不是短，而是把三件事混在一起了：
- 逻辑设计
- 范围规划
- 实际落地

Claude 只能边猜边做。

更稳一点的写法会像这样：

```text
Think through a comments feature for this app.
Include auth, moderation, and real-time updates.
List the main risks and trade-offs first.
Do not implement yet.
```

然后第二轮再说：

```text
Now turn that into an implementation plan.
List the files that would likely change.
Still do not edit anything yet.
```

这样你至少先把“想清楚”和“改出去”拆开了。

---

## 我更推荐的实操顺序

如果你遇到一个不小的任务，我建议按这个顺序：

1. 先判断它是“范围问题”还是“复杂度问题”
2. 范围大就 planning
3. 逻辑复杂就 thinking
4. 如果两者都有，先 think，再 plan
5. 计划通过后再动代码
6. 动代码时仍然保留 review

这看起来比“一句话扔给 AI”慢一点。

但在真实项目里，通常更快。因为它显著减少返工。

---

## 最容易踩的坑

### 1. 看到 plan 就默认放心
不对。plan 只是第一版实施想法，不是最终正确答案。

### 2. 把 Planning 当 Thinking 用
计划列得再清楚，也不代表逻辑已经想明白。

### 3. 把 Thinking 当成“更强模型模式”
Thinking 会增加时间和 token 成本，不是每次都该开。

### 4. 权限越开越大
一旦为了图快把 edit / bash 放得太松，后面就容易失控。

---

## 我的判断

第 4、5 集最关键的，不是教你几个功能名，而是让你开始像管一个协作者那样管 Claude Code：

- 给它合适的权限
- 让它先想清楚再行动
- 大任务不要一次性扔出去

说白了，Claude Code 不是不能做大任务，而是**你不能用小任务的管理方式去管大任务。**
