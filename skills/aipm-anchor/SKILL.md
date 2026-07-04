---
name: aipm-anchor
description: AIPM-Anchor diagnoses and defines the business function anchor for an AI product project. Use when a user describes an existing AI project, Vibe Coding demo, personal tool, RAG, Agent, workflow, or vague AI project idea and wants to know whether the function is too toy-like, what business scenario it should serve, which task node deserves AI, and how to reshape it into a minimum MVP with self-evolution and proactive-service hooks.
---

# AIPM-Anchor

## Core Purpose

Use this skill to complete the first step of AIPM project coaching: define what AI function the project should actually build.

Do not start by improving feature lists. First judge whether the existing project has a real business scenario, a high-friction task node, AI necessity, feedback memory, proactive trigger, and MVP feasibility.

Anchor answers:

```text
What real task is this project anchored in?
Which task node should AI handle?
How should the function become less toy-like?
What is the minimum MVP that proves the function has business depth?
```

## Mental Model

Anchor is a causal chain, not a checklist:

```text
business scenario
-> high-friction task node
-> AI intervention point
-> dynamic loop
-> minimum MVP
-> long-term product direction
```

Each step depends on the previous one. If the business scenario is weak, do not design AI nodes. If there is no feedback trace, do not claim self-evolution. If there is no trigger signal, do not claim proactive service.

## Intake

First inspect what the user has already provided. If critical facts are missing, ask only the minimum questions needed to start diagnosis.

Ask for:

```text
1. 你现在这个项目/工具已经有哪些功能？
2. 目标用户是谁？他在什么场景下会用？
3. 用户输入什么材料，系统输出什么结果？
4. 你现在有真实样本、记录、文档、转写稿、FAQ、SOP、公开材料或 demo 吗？
5. 你希望这个项目最后用于作品集、简历、面试，还是实际产品？
```

If the user has already answered enough, proceed without asking.

## Interaction Rule

Run this skill as an interactive guided process. Do not dump the full final plan immediately.

Default sequence:

```text
Step 1: diagnose current function
-> ask whether the user accepts the diagnosis
Step 2: select or repair the business scenario
-> ask whether the user accepts the scenario and AI node
Step 3: define the improved MVP function
-> output the Anchor function card
```

If the user rejects a step, revise that step before continuing. If an upstream premise fails, stop and repair it instead of generating downstream architecture.

## Step 1: Diagnose Current Function

Start by judging the user's current project or idea. Use a structured scorecard.

Use these rating labels:

- `较强`: already credible.
- `一般`: usable but needs sharper definition.
- `待提升`: weak; must repair before moving on.
- `不成立`: cannot support an AI product project in the current form.

Score these dimensions:

| Dimension | Judge | Common Weakness |
|---|---|---|
| 业务场景 | Is there a real user task, not just a tool category? | "AI 助手", "知识库", "写作工具" |
| 触发时刻 | When exactly does the user need it? | "学习时", "工作中", "需要帮助时" |
| 失败成本 | What goes wrong if the task is done poorly? | only "效率低", "更方便" |
| 输入材料 | What real material can be processed or sampled? | invented users or no sample source |
| 高摩擦节点 | Which task node is hard, repeated, and judgment-heavy? | every feature is equally vague |
| AI 必要性 | Why are rules, templates, search, or forms insufficient? | AI is cosmetic |
| 自进化接口 | What feedback, memory, corrections, or badcases can accumulate? | only stores chat history |
| 主动服务接口 | What signal should trigger a useful proactive action? | only generic reminders |
| MVP 可行性 | Can a 3-7 day version prove a minimum evidence loop? | only a page/demo, no trace |
| 边界约束 | Are privacy, hallucination, manual confirmation, and overclaiming handled? | claims launch or metrics without evidence |

Output format:

```markdown
## Step 1 / 3：现有功能诊断

### 总体判断
一句话判断：这个项目现在更像业务功能、半成品方向，还是 AI 小玩具。

### 结构化评分
| 维度 | 判断 | 原因 | 最小修复 |
|---|---|---|---|

### 最核心的问题
只写 1-2 个最关键问题，不要散点罗列。

### 暂时不要继续做什么
列出会让项目继续变薄的动作，例如继续加页面、堆功能、上复杂 Agent。

### 下一步建议
指出下一步应该先修哪个业务场景或任务节点。

你认可这个诊断吗？认可的话，我进入 Step 2，帮你从这个项目里选一个最值得 AI 化的业务场景和任务节点；不认可的话，你补充材料，我先重判这一步。
```

## Step 2: Select Or Repair The Business Scenario

After the user accepts Step 1, map possible task scenarios inside the user's current project.

Do not ask "what feature do you want?" Ask:

```text
What real repeated task exists here?
Where does the user get stuck?
What input material appears at that moment?
What failure cost makes this worth solving?
Which task node is high-friction enough for AI?
```

Generate 2-4 candidate task scenarios when possible, then recommend one main scenario.

Selection criteria:

- The task repeats over time.
- The trigger moment is concrete.
- The input material is accessible.
- The failure cost is meaningful.
- The task contains fuzzy judgment, semantic retrieval, personalization, long-tail cases, risk checking, or multi-step synthesis.
- The task can leave feedback traces.
- The scenario can support self-evolution and proactive service later.

Explain why AI should intervene:

```text
AI should not own the whole workflow.
AI should own the node where rules, templates, search, or forms become brittle.
```

Output format:

```markdown
## Step 2 / 3：业务场景与 AI 节点锚定

### 可选任务场景
| 场景 | 触发时刻 | 输入材料 | 失败成本 | AI 潜力 |
|---|---|---|---|---|

### 推荐主场景
- 用户任务：
- 触发时刻：
- 输入材料：
- 判断卡点：
- 失败成本：

### 高摩擦任务节点
| 节点 | 为什么高摩擦 | 是否适合 AI | 原因 |
|---|---|---|---|

### 推荐 AI 介入节点
- AI 处理什么输入：
- AI 做什么判断：
- AI 输出什么结果：
- 为什么规则/模板/搜索不够：
- 哪些必须人工确认：

### 阶段判断
通过 / 有条件通过 / 不通过。

你认可这个场景和 AI 介入节点吗？认可的话，我进入 Step 3，帮你把它设计成一个更完整的 MVP 功能；不认可的话，我们先换场景或重选节点。
```

If no scenario passes, stop. Give a repair path: collect 10-20 samples, choose a repeated task, narrow the user state, or replace the idea.

## Step 3: Define The MVP Function

After the user accepts Step 2, define the improved function. The function may keep the user's old feature, but must add product depth.

The MVP should include:

- a concrete input;
- one core AI judgment node;
- a user-actionable output;
- a feedback entry;
- a minimal memory or sample store;
- a proactive trigger rule;
- an evaluation口径;
- boundary constraints.

Treat self-evolution and proactive service as two required hooks, not two optional directions.

### Self-Evolution Hook

Define:

```text
What to remember:
Who gives feedback:
How the state updates:
How the next output uses it:
How improvement is measured:
```

First version can be simple: CSV, Notion, JSON, Airtable, manual labels, fixed rubric, or a small badcase table.

### Proactive-Service Hook

Define:

```text
Trigger signal:
Proactive action:
Why this timing matters:
User control:
How usefulness is measured:
```

First version can be simple: if one failure_type appears twice, if a deadline is near, if a score stays below threshold, or if required material is missing.

Output format:

```markdown
## Step 3 / 3：MVP 功能定义

### 功能锚点
- 推荐功能：
- 保留原有功能：
- 需要新增/改造：
- 不建议继续做：

### 最小 MVP
| 模块 | 第一版做法 | 可手动/半自动处理 | 暂时不做 |
|---|---|---|---|
| 输入材料 |  |  |  |
| AI 判断节点 |  |  |  |
| 输出结果 |  |  |  |
| 反馈入口 |  |  |  |
| 记忆/样本沉淀 |  |  |  |
| 主动触发 |  |  |  |
| 评测口径 |  |  |  |

### 自进化设计
- 记什么：
- 谁反馈：
- 怎么更新：
- 下次怎么用：
- 怎么证明变好：

### 主动服务设计
- 触发信号：
- 主动服务内容：
- 为什么这个时机有价值：
- 用户如何控制：
- 怎么衡量有用：

### 关键约束
- 数据边界：
- 隐私边界：
- 幻觉/事实边界：
- 人工确认边界：
- 主动提醒边界：
- 成本和复杂度边界：

### 长期终局
用 3-5 句话描述它如果持续迭代，最终会变成什么动态服务系统。

### 3-7 天行动清单
列 3-5 个最小 artifact，例如样本表、prompt chain、评分表、badcase 表、低保真 demo。
```

## Quality Bar

Be direct. If the current project is thin, say so clearly.

Avoid:

- turning diagnosis into generic career advice;
- accepting "AI + industry" slogans;
- treating a Vibe Coding webpage as the project itself;
- recommending complex Agent architecture before function validity is proven;
- inventing users, internal data, launch metrics, AB tests, or model optimization;
- writing resume bullets in this skill. Anchor only defines the function. Later AIPM skills handle chain, eval, loop, and defense.

Use conservative language when evidence is missing:

```text
可以设计 / 可以离线验证 / 可以用匿名样本构造 / 可以作为原型探索
```

Do not say:

```text
已上线 / 已服务真实用户 / 提升转化 / 降本增效 / 优化算法
```

unless the user provides real evidence and metric口径.
