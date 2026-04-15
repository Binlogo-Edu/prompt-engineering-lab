# Tree of Thoughts

## Status

Completed on 2026-04-15 after concept check.

## Core idea

Tree of Thoughts（ToT）把问题求解看成一棵**搜索树**，而不是一条线性的推理链。  
每个 `thought` 都是一段有意义的中间步骤；模型不只沿着一条路径一直往下写，而是会：

1. 生成多个候选 thought
2. 评估哪些 thought 更有希望
3. 继续展开更优分支
4. 必要时回退（backtrack）并探索别的路径

一句话：**CoT 是一条链，ToT 是一棵树。**

## 为什么需要 ToT

CoT 已经比直接回答更强，但它仍然有两个明显限制：

- **局部上不探索**：某一步一旦选错，后面整条链都会被带偏
- **全局上不规划**：没有系统性的前瞻、比较和回退机制

Self-Consistency 虽然会采样多条完整推理链再投票，但它仍然是**链与链之间的比较**，不是在中间步骤上进行细粒度搜索。

ToT 要解决的问题正是：

- 不要太早把自己锁死在单一路径上
- 让模型在中间步骤就进行筛选，而不是等整条链写完才发现方向错了

## ToT 的四个核心组成

根据论文，ToT 的一个具体实现通常要回答四个问题。

### 1. Thought decomposition

先定义"一步 thought"到底有多大。

- 在 `Game of 24` 里，thought 可以是一行中间算式
- 在创意写作里，thought 可以是一段写作提纲
- 在填字游戏里，thought 甚至可以是一个词

原则：thought 要足够小，便于分叉探索；也要足够大，便于评估价值。

### 2. Thought generation

从当前状态生成下一步候选 thought，常见两种方式：

- **Sample**：独立采样多个候选，适合 thought 空间很丰富的任务
- **Propose**：按当前上下文顺序提出候选，适合约束更强的任务

### 3. State evaluation

生成多个候选后，要判断哪些状态更值得继续探索。论文给出两种思路：

- **Value**：分别给每个状态打分，例如 `sure / likely / impossible`
- **Vote**：把多个状态放在一起比较，让模型投票选更有前景的分支

这里评估的不是最终答案，而是"这个中间状态是否值得继续走下去"。

### 4. Search algorithm

有了候选和评分后，就要决定如何搜索这棵树。论文主要讨论两种：

- **BFS（广度优先）**：每一层保留若干最有希望的状态继续扩展
- **DFS（深度优先）**：优先沿着最 promising 的状态往下走，不行再回退

所以 ToT 不是单纯的 prompt 技巧，它已经带有明显的**搜索/规划框架**色彩。

## 一个直观例子

论文里的 `Game of 24` 示例输入是 `4, 9, 10, 13`，目标是组成 `24`。

如果用普通 CoT，模型可能一条路走到底，算错就结束。  
如果用 ToT，可以把中间状态拆成多步：

1. 先尝试不同的第一步变换
2. 例如发现 `13 - 9 = 4`，剩下 `4, 4, 10`
3. 再继续探索，得到 `10 - 4 = 6`，剩下 `4, 6`
4. 最后 `4 * 6 = 24`

关键不在于这条路径本身，而在于：

- 模型可以同时考虑多种第一步
- 可以先比较哪些中间状态更接近可解
- 如果某条路径明显无望，可以剪枝或回退

## ToT 和前面几章的关系

| 方法 | 结构 | 主要能力 | 主要局限 |
|---|---|---|---|
| Zero-shot | 直接回答 | 快、简单 | 复杂推理不稳 |
| Few-shot | 示例驱动 | 学格式、学模式 | 遇到复杂推理仍会卡住 |
| CoT | 单条推理链 | 展开中间步骤 | 一条路走偏就完 |
| Self-Consistency | 多条链 + 投票 | 提高确定性推理准确率 | 不做局部搜索，成本高 |
| ToT | 多分支树搜索 | 规划、前瞻、回退、剪枝 | 成本更高，实现更复杂 |

可以把它们理解成一条增强路径：

`CoT` 解决"要不要写中间步骤"  
`Self-Consistency` 解决"单条链不可靠怎么办"  
`ToT` 解决"能不能在中间步骤上做搜索和规划"

## 适用场景

ToT 更适合这些任务：

- 需要规划和搜索的任务
- 早期错误会显著影响后续结果的任务
- 存在多条候选路径、需要比较中间状态优劣的任务
- 谜题、组合搜索、复杂推理、结构化创作规划

论文中的代表任务包括：

- `Game of 24`
- `Creative Writing`
- `Mini Crosswords`

## 局限性

- **成本高**：要生成多个分支，还要评估和搜索，Token 与延迟都明显上升
- **容易树爆炸**：分支数一大，搜索空间会迅速膨胀
- **评估器未必可靠**：如果模型对中间状态判断失真，可能把好分支剪掉
- **不适合简单任务**：问题本来就直给时，ToT 往往是过度设计

所以 ToT 不是"默认更强"，而是**在复杂规划/搜索任务中值得付费使用**。

## My summary

- ToT 把 LLM 推理从"单链生成"升级为"基于 thought 的树搜索"
- thought 是中间语义单元，不是逐 token 的盲目延伸
- 它的关键不是多生成一点，而是：**生成、评估、保留、回退**
- Self-Consistency 是多条完整链的投票；ToT 是中间步骤级别的搜索
- 它更像一种 `reasoning + search` 框架，而不只是提示词表面写法

## Self-test

1. Tree of Thoughts 和 Chain-of-Thought 最核心的结构差异是什么？
2. ToT 为什么比 Self-Consistency 更适合需要规划的任务？
3. ToT 的四个关键组成部分分别是什么？
4. 什么情况下应该用 BFS，什么情况下更可能考虑 DFS？
5. ToT 为什么不能当成所有任务的默认方案？

## Next step

After concept check, move on to RAG.

## Review result

Self-test passed on 2026-04-15.

What I got right:

- ToT is not just "more reasoning"; it explicitly introduces planning through branching, evaluation, and search
- it is better suited than Self-Consistency for planning tasks because it selects paths during reasoning, not only answers at the end
- the four key components are thought decomposition, thought generation, state evaluation, and search algorithm
- ToT should not be the default because search is expensive and unnecessary for simple direct tasks
