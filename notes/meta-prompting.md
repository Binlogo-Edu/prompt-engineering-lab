# Meta Prompting

## Status

Completed on 2026-04-04 after concept check.

## Core idea

Meta Prompting 关注的是任务的**结构和语法形式**，而不是具体内容。它用抽象的模板来引导模型，告诉模型"问题长什么样、答案应该什么结构"，而不是给出具体的内容示例。

## 与 Few-shot 的核心区别

| | Few-shot | Meta Prompting |
|---|---|---|
| 关注点 | 具体内容（示例输入输出） | 抽象结构（格式和模式） |
| 示例类型 | 真实的带内容的例子 | 抽象的结构模板 |
| Token 消耗 | 较高（需要完整示例） | 较低（只描述结构） |
| 本质 | 内容驱动 | 结构驱动 |

## 五个关键特征（Zhang et al. 2024）

1. **Structure-oriented（结构导向）**：优先考虑问题和答案的格式与模式，而非具体内容
2. **Syntax-focused（语法聚焦）**：用语法作为期望回答的模板
3. **Abstract examples（抽象示例）**：用抽象框架说明结构，不聚焦具体细节
4. **Versatile（通用性强）**：可跨领域应用，适合各类结构化任务
5. **Categorical approach（分类方法）**：借鉴类型理论，强调组件的分类和逻辑排列

## 优势

- **Token 效率高**：只描述结构而非内容，消耗更少 Token
- **更公平的对比基准**：减少具体示例对模型的影响，更适合评估模型本身能力
- **接近 Zero-shot**：可以看作一种 zero-shot 变体，最小化了具体示例的影响

## 局限性

- 依赖模型对任务的**先验知识**：Meta Prompting 假设模型已经了解目标任务
- 对于**新颖或罕见任务**，效果可能下降（与 zero-shot 类似的局限）

## 适用场景

- 复杂推理任务
- 数学问题求解
- 编程挑战
- 理论性问题

## 与其他技术的关系

```
Zero-shot ──→ 没有示例，直接指令
Few-shot  ──→ 有内容示例，内容驱动
Meta      ──→ 有结构模板，结构驱动（更接近 zero-shot）
CoT       ──→ 在示例中加入推理步骤
```

## Self-test

1. Meta Prompting 和 Few-shot 最核心的区别是什么？
2. 为什么 Meta Prompting 比 Few-shot 更节省 Token？
3. Meta Prompting 为什么可以被看作一种 Zero-shot 变体？
4. Meta Prompting 在什么情况下会失效？
5. 如果要解决一个数学推导题，Meta Prompting 和 CoT 你会怎么选？

## Review result

Self-test passed on 2026-04-04.

What I got right:

- Few-shot 是内容驱动，Meta Prompting 是结构驱动
- Meta Prompting 节省 Token 是因为把"从示例归纳结构"的工作转移给了写提示词的人，模型只接收结构指令
- 没有具体内容示例 → 符合 zero-shot 定义，Meta Prompting 本质是结构化的 zero-shot
- 失效条件：模型缺乏目标任务的先验知识时（类似 zero-shot 的局限）
- 数学推导选 CoT：因为需要每一步计算都正确，CoT 让错误可在中间步骤被发现；Meta Prompting 只约束形式，不保证推导过程正确

## Next step

Move on to Self-Consistency prompting.
