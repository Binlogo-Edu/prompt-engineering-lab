# Retrieval-Augmented Generation

## Status

Completed on 2026-07-18 after concept check and design practice.

## Core idea

Retrieval-Augmented Generation（RAG）不是把新知识训练进模型，而是先从外部知识库检索相关资料，再把这些资料放进 prompt，让模型基于当前上下文回答。

一句话：**RAG 改变的是模型获取信息的方式，不是模型参数本身。**

它特别适合需要当前、私有、可追溯知识的场景，例如公司制度问答、产品文档问答、客服知识库、代码库问答和法律/医疗等高准确性资料查询。

## Standard pipeline

一个典型 RAG 流程是：

```text
Documents
→ Chunking
→ Embedding
→ Retrieval
→ Prompt with Context
→ Generation
→ Citation / Unknown when unsupported
```

### 1. Documents

知识库应放权威、可作为依据的资料，例如：

- product docs
- internal policy documents
- FAQ
- manuals
- database records
- source code
- web pages

关键不是资料越多越好，而是资料要可信、可更新、可追溯。

### 2. Chunking

RAG 通常不会把整篇文档直接塞给模型，而是先把文档切成 chunks，再按问题检索相关片段。

chunk size 的核心取舍：

| Chunk size | Benefit | Risk |
|---|---|---|
| Too large | keeps more context | adds noise, wastes context window, makes retrieval less precise |
| Too small | matches narrowly | loses surrounding context and may only recall partial evidence |

一句话：**chunk 太大，相关性变差；chunk 太小，完整性变差。**

实际系统中常配合使用：

- `chunk overlap`：避免关键内容被切断
- `metadata`：保留标题、章节、来源、更新时间、适用范围
- `reranking`：先粗召回，再重新排序，减少相似但无关的内容进入 prompt

### 3. Embedding

Embedding 把文本转换成向量，使系统可以计算语义相似度。

例如用户问：

```text
怎么重置密码？
```

知识库可能写的是：

```text
用户可以通过账户安全页面重新设置登录凭证。
```

关键词搜索可能匹配不上，因为字面词不同；embedding 检索更可能找到它，因为两者语义相近。

但 embedding 的风险是：它找的是**语义相似**，不是**事实正确**。例如“注销账户”和“退出登录”语义相关，但实际任务不同。

### 4. Retrieval

Retriever 决定哪些 chunks 会被带进 prompt，因此它直接影响答案质量。

RAG 的一个重要判断：

> RAG 的答案质量上限，首先受检索质量限制。

常见检索失败包括：

- 没有召回正确资料
- 召回了相似但错误的资料
- `top_k` 太小导致漏召回
- `top_k` 太大导致噪声进入上下文
- metadata filter 误过滤了正确 chunk
- 用户问题需要改写或补充条件后才能检索

`top_k` 不是越大越好。它需要平衡：

- **Recall**：正确资料有没有被找出来
- **Precision**：找出来的资料里，有多少是真的相关

### 5. Generation

生成阶段的重点是让模型基于检索资料回答，而不是凭训练数据或常识补全。

常见 prompt 约束：

```text
Use only the provided context to answer the question.
If the answer is not in the context, say you do not know.
Cite the relevant source when possible.
Do not invent company policy or product behavior that is not stated in the context.
```

这条约束主要控制的是 unsupported answer 风险：答案看起来合理，但没有被当前检索资料支持。

## Debugging RAG failures

RAG 出错时，不能只看最终答案，因为最终答案只是症状。

需要同时检查：

| Layer | Possible issue | How to inspect |
|---|---|---|
| Retrieval | correct evidence was not retrieved, or wrong evidence was retrieved | inspect retrieved chunks |
| Prompt assembly | correct evidence was retrieved but truncated, buried, or mixed with conflict | inspect final context sent to model |
| Generation | evidence was present but model ignored or misread it | compare answer against context |

一个实用原则：

> 对每个错误答案，先打印 retrieved chunks，看模型到底看到了什么。

如果 retrieved chunks 里没有答案，这是检索问题；如果 chunks 有答案但模型没答对，才更可能是生成问题。

## Example: internal leave-policy QA bot

设计一个公司内部请假政策问答机器人时，可以这样拆：

### Knowledge base

放权威政策资料：

- employee handbook
- annual leave, sick leave, personal leave, marriage leave, maternity leave rules
- approval workflow
- regional or employee-type-specific rules
- HR FAQ
- source link, updated date, applicable scope

### Chunking

不要把整篇政策直接作为一个 chunk。更合理的是按语义单元切分：

- Annual leave rules
- Sick leave proof requirements
- Personal leave limits
- Marriage leave duration
- Maternity and paternity leave rules
- Approval workflow
- Leave system instructions

每个 chunk 最好能独立回答一个具体问题，同时保留标题、来源、更新时间和适用范围。

### Retrieval

用户问：

```text
我生病请一天假需要病假证明吗？
```

系统应优先召回：

- sick leave rules
- sick leave proof requirements
- leave approval workflow

如果政策按地区或员工类型不同，还需要使用 metadata filter 或追问补充条件。

### Answer constraints

回答时要求模型：

- only answer from retrieved policy chunks
- cite policy title and updated date
- say the current documents do not specify the answer when evidence is missing
- suggest contacting HR for unresolved policy gaps
- avoid using general assumptions about leave policies

## My summary

- RAG 的本质是把外部知识临时注入上下文，让模型基于可更新、可追溯的资料回答
- RAG 不改变模型参数，但会强烈影响当次输出
- 检索器找错资料，模型就可能基于错误上下文生成错误答案
- chunking 决定检索粒度，embedding 决定语义匹配，retriever 决定上下文质量
- 召回更多 chunk 不一定更好，因为噪声会挤占上下文并误导模型
- RAG 调试要先看 retrieved chunks，再看最终答案

## Self-test

1. RAG 为什么不是把知识训练进模型？
2. chunk 太大和太小分别有什么问题？
3. embedding 检索为什么能匹配字面不同但语义相似的内容？
4. 为什么 `top_k` 不是越大越好？
5. RAG 出错时为什么要检查 retrieved chunks？
6. 在公司内部政策问答场景里，如何避免模型凭常识编造答案？

## Review result

Concept check passed on 2026-07-18.

What I got right:

- RAG changes information access by retrieving external knowledge and injecting it into the prompt
- retrieved context influences the current answer without changing model weights
- wrong retrieval can mislead generation even when the user question is valid
- chunk size trades off context completeness and retrieval precision
- embedding retrieval is semantic similarity, not factual correctness
- more chunks can increase noise and reduce answer quality
- RAG debugging should inspect retrieved chunks to separate retrieval failure from generation failure

## Next step

Move on to ReAct.
