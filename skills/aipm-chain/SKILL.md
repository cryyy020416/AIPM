---
name: aipm-chain
description: AIPM-Chain turns an AI product function anchor into a concrete implementation chain. Use after AIPM-Anchor or when a user already has one or more AI MVP function links and needs to choose a main link, map the end-to-end loop, then drill into each step's input, processing, output, boundary conditions, data/state persistence, MVP implementation, and minimal technology choices without teaching generic technical theory.
---

# AIPM-Chain

## Core Purpose

Use this skill to complete the second step of AIPM project coaching: translate an anchored AI function into an implementation chain.

Chain answers:

```text
Which MVP function link should be implemented first?
How does this link run from trigger to next action?
For each step, what is the input, processing, output, boundary, and persisted state?
Which parts need AI, and which can be rules, tables, manual review, or simple storage?
What is the smallest implementation path that proves the link can run?
```

Do not teach broad technical concepts. Do not start from RAG, Agent, vector database, React, backend, or model choice. Start from the task chain.

## Mental Model

Chain is a recursive implementation lens:

```text
main function link
-> end-to-end loop
-> step
-> node inside the step
-> input / processing / output / boundary / persistence
```

The same questions repeat at every layer:

```text
What enters this part?
What happens here?
What comes out?
What can go wrong?
What should be saved?
What is enough for the first version?
```

The output should feel like one traceable chain, not many unrelated tables. Use stable IDs such as `L1`, `L2`, `L3` for main steps and `L3.1`, `L3.2` for nodes inside a step.

## Intake

First inspect what the user has already provided. If they provide the output of AIPM-Anchor, use it directly.

Minimum required inputs:

```text
1. 项目名 / 一句话定义
2. 第一版要解决的问题
3. MVP 功能链路列表
4. 推荐 AI 介入节点
5. 自进化接口
6. 主动服务接口
7. 关键约束
```

If these are missing, ask only for the minimum needed. If the user has several function links, do not expand all of them. First choose one main link.

## Interaction Rule

Run this skill as an interactive guided process. Do not dump the full technical implementation card immediately.

Default sequence:

```text
Step 1: select the main function link
-> ask whether the user accepts this link
Step 2: map the end-to-end loop for that link
-> ask whether the user wants to drill down
Step 3: drill into each step's input, processing, output, boundary, persistence, and MVP implementation
-> output the Chain implementation card
```

If the user says they only want a high-level chain, stop after Step 2. If they want more detail, continue to Step 3. If the selected link is wrong, return to Step 1 instead of refining downstream details.

## Step 1: Select The Main Function Link

When the previous Anchor output contains multiple MVP function links, choose the one most suitable for first implementation.

Selection criteria:

- It proves the core user value.
- It contains the main AI judgment node.
- It has clear input and output.
- It can collect feedback or bad cases.
- It can support a minimal memory/state mechanism.
- It can connect to a proactive trigger later.
- It can be implemented as a 3-7 day evidence loop.

Output format:

```markdown
## Step 1 / 3：主链路选择

### 可选功能链路
| 功能链路 | 解决的问题 | 输入是否明确 | AI 节点是否明确 | 是否能沉淀反馈 | 第一版可行性 |
|---|---|---|---|---|---|

### 推荐先做的主链路
- 主链路：
- 为什么先做它：
- 暂时不先做哪些链路：
- 为什么暂时不做：

### 这一阶段的边界
- 第一版只展开：
- 第一版不展开：

你认可先拆这条主链路吗？认可的话，我进入 Step 2，先把它从触发到下一次服务的闭环梳理出来；不认可的话，我们先换主链路。
```

## Step 2: Map The End-To-End Loop

After the user accepts the main link, map the vertical loop first. Keep it macro-level. Do not drill into every node yet.

Use this default chain:

```text
trigger
-> input
-> preprocessing
-> core processing
-> output
-> feedback
-> state persistence
-> next use / proactive trigger
```

Use Chinese labels in delivery:

```text
触发
-> 输入
-> 预处理
-> 核心处理
-> 输出
-> 反馈
-> 状态沉淀
-> 下一次/主动触发
```

Output format:

```markdown
## Step 2 / 3：主链路闭环梳理

### 主链路一句话
这条链路把「A 输入」经过「B 判断/处理」转成「C 输出」，并通过「D 反馈/状态」影响下一次服务。

### 主功能链路地图
| 环节 ID | 环节名称 | 这一环节解决什么 | 用户/系统动作 | 进入下一环节的结果 |
|---|---|---|---|---|
| L1 | 触发 |  |  |  |
| L2 | 输入 |  |  |  |
| L3 | 预处理 |  |  |  |
| L4 | 核心处理 |  |  |  |
| L5 | 输出 |  |  |  |
| L6 | 反馈 |  |  |  |
| L7 | 状态沉淀 |  |  |  |
| L8 | 下一次/主动触发 |  |  |  |

### 目前最关键的实现判断
只写 2-4 条，例如：哪个环节必须 AI，哪个环节先手动，哪个环节必须留下状态。

你要我继续往下拆每个环节的「输入、处理、输出、边界、沉淀和第一版实现」吗？如果继续，我会按 L1-L8 逐层下钻；如果不继续，这一步可以先停在主链路地图。
```

If the user declines further detail, stop. Provide only a concise summary of the chain and next recommended action.

## Step 3: Drill Into Each Step

After the user agrees to drill down, expand the same chain with the same IDs.

Do not create disconnected tables. Step 3 should read like a traceable implementation explanation mounted to the same `L1-L8` chain. Use the `环节 ID` and `环节名称` from Step 2 as the spine, then drill down with short headings and numbered lists.

```text
环节 ID
环节名称
```

### 3.1 Per-Step Narrative Drilldown

For every `L` step, use this structure:

```markdown
### L1 触发

先用 1-2 句话说明这个环节在主链路里负责什么，以及为什么它存在。

**输入可能来自几类情况：**

1. ...
2. ...
3. ...

**处理动作：**

1. ...
2. ...
3. ...

**实现方式取舍：**

1. 第一版可以用 ...，因为 ...
2. 后续只有在 ... 条件出现时，才升级到 ...

**输出、状态和边界：**

这个环节输出 ...，交给 L2 ...。需要保存 ...。

失败边界是 ...。第一版用 ... 兜底。MVP 证据是 ...。
```

This answers:

```text
This step eats what?
This step does what?
This step produces what?
Who uses the result?
What state or evidence should be saved?
Where can it fail?
What is enough for the first version?
```

### 3.2 Input Cases

For each step, make input cases visible. Do not bury them in one long paragraph.

Use wording such as:

```text
输入可能来自三类情况：
输入通常包括五类：
输入可能有四种状态：
```

Then enumerate the cases:

```markdown
1. 用户手动提交的材料。
2. 系统从历史状态里带出的材料。
3. 咨询师为了 MVP 演示人工整理出的材料。
```

### 3.3 Processing Actions And Implementation Choices

Split processing into concrete actions before discussing tools.

Good:

```markdown
**处理动作：**

1. 校验必填字段。
2. 提取平台约束。
3. 判断哪些素材要补充。
```

Then explain implementation choices in plain and minimal terms:

- manual / semi-manual review;
- table, CSV, Notion, Airtable, JSON;
- fixed rules or thresholds;
- prompt chain;
- retrieval only when evidence or historical cases matter;
- database only when state must persist across many cases;
- background task only when proactive triggers need automation;
- agent only when there are genuinely dynamic multi-tool operations.

Use this wording:

```text
这个节点要做的是 X。
第一版用 Y 就够，因为 Z。
后续只有在 A 条件出现时，才升级到 B。
```

### 3.4 Output, Persistence, Boundary, And MVP Evidence

End every step with one compact paragraph titled:

```markdown
**输出、状态和边界：**
```

This paragraph must include:

- output: what this step produces and who receives it;
- persistence: what should be saved, versioned, reviewed, or desensitized;
- boundary: what can fail and how the first version catches or reduces the damage;
- MVP evidence: what proves this step is working enough for the first version.

Boundary categories to consider:

- input missing or low-quality;
- evidence missing;
- wrong retrieval;
- weak or hallucinated judgment;
- vague output;
- feedback too costly for user;
- dirty memory/state;
- proactive trigger false positive;
- privacy or sensitive data;
- cost and over-complexity.

The MVP is not a full build plan. It is the smallest evidence loop:

```text
one runnable main link;
10-20 sample cases;
one structured input/output record;
one feedback or badcase table;
one state that affects next use;
one lightweight proactive trigger;
one clear explanation of how the chain works.
```

### 3.5 Default Step Labels

Unless the user's project clearly needs different labels, drill down these eight steps:

```text
L1 触发
L2 输入
L3 预处理
L4 核心处理
L5 输出
L6 反馈
L7 状态沉淀
L8 下一次/主动触发
```

Keep simple steps short. Expand `预处理`, `核心处理`, `反馈`, `状态沉淀`, and `主动触发` more deeply when they contain the main implementation uncertainty.

## Final Output Format

When completing Step 3, output:

```markdown
## Step 3 / 3：主链路实现下钻

### L1 触发

...

**输入可能来自三类情况：**

1. ...
2. ...
3. ...

**处理动作：**

1. ...
2. ...

**实现方式取舍：**

1. ...
2. ...

**输出、状态和边界：**

...

### L2 输入
...

### L3 预处理
...

### L4 核心处理
...

### L5 输出
...

### L6 反馈
...

### L7 状态沉淀
...

### L8 下一次/主动触发
...

### 3-7 天最小证据闭环
- 第 1 个证据：
- 第 2 个证据：
- 第 3 个证据：
- 暂时不做：
```

## Quality Bar

Be concrete, conservative, and chain-first.

Avoid:

- expanding all function links at once;
- turning Chain into a technical lecture;
- using RAG, Agent, vector database, fine-tuning, or backend terms before the chain requires them;
- creating disconnected tables with different first columns;
- inventing data, users, metrics, integrations, or deployment facts;
- producing a full engineering sprint plan before the implementation chain is clear;
- claiming self-evolution if history does not affect future output;
- claiming proactive service if no trigger signal and user control exist.

Use conservative language:

```text
第一版可以手动模拟 / 可以离线验证 / 可以用表格沉淀 / 可以用固定规则触发 / 后续再自动化
```

Do not say:

```text
自动学习 / 实时监控 / 完整 Agent 系统 / 已经形成闭环 / 可规模化上线
```

unless the user provides evidence and the implementation chain actually supports it.
