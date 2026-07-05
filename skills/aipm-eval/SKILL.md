---
name: aipm-eval
description: AIPM-Eval turns an AI product implementation chain into a practical evaluation system. Use after AIPM-Chain or when a user already has an AI project main chain and needs to define evaluation purpose, evaluation objects, offline eval cases, MECE rubrics, human/LLM judging workflow, illustrative bad-case diagnosis, retest criteria, and safe project/resume wording without overclaiming online impact.
---

# AIPM-Eval

## Core Purpose

Use this skill to complete the third step of AIPM project coaching: build an evaluation system for one AI product chain.

Eval answers:

```text
What exactly should be evaluated?
Which samples prove whether this chain works?
Which rubric tells good output from bad output?
When a score is low, which chain node should be changed?
How can the user retest with the same criteria after iteration?
What can and cannot be claimed in project materials?
```

Do not teach generic model evaluation. Do not start from accuracy, BLEU, benchmark scores, online metrics, or A/B testing. Start from the user's `AIPM-Chain` main link and ask: "This AI function is supposed to solve which task, and how do we know when it fails?"

## Mental Model

Eval is a validation chain:

```text
Chain main link
-> evaluation purpose
-> evaluation object
-> eval cases
-> MECE rubric
-> human/LLM judging
-> bad-case diagnosis examples
-> same-criteria retest
```

Every metric must map back to either:

1. a chain node from the second stage, such as `L3`, `L4`, or `L5`; or
2. an end-to-end user task result.

If a metric cannot map back to the chain or user task, do not include it.

## Intake

First inspect what the user has already provided. If they provide the output of AIPM-Chain, use it directly.

Minimum required inputs:

```text
1. 项目名 / 一句话定义
2. 第二阶段主链路名称
3. L1-L8 链路地图
4. AI 介入节点
5. 输入 / 输出
6. 失败边界
7. 目前是否有真实样本、公开样本、脱敏材料或 demo 输出
```

If the chain is missing, ask for only the minimum needed. Do not invent chain IDs, user logs, sample counts, online behavior, or real results.

## Interaction Rule

Run this skill as an interactive guided process. Do not dump the full evaluation system immediately.

Default sequence:

```text
Step 1: inherit Chain and define evaluation purpose
-> ask whether the user accepts the evaluation positioning
Step 2: choose evaluation objects and draft evaluation dimensions
-> ask whether the user accepts what should be measured
Step 3: design eval set, eval-case table, and MECE rubric
-> ask whether the user wants illustrative bad-case diagnosis and retest examples
Step 4: provide illustrative bad-case diagnosis examples, retest criteria, and safe wording
-> output the Eval system card
```

If the user rejects a step, revise that step before continuing. If the Chain premise is unclear, stop and repair the chain instead of generating downstream evaluation details.

## Step 1: Inherit Chain And Define Evaluation Purpose

Use the second-stage chain as the evaluation spine.

Check:

- Which nodes most affect output quality?
- Which nodes contain AI judgment, retrieval, routing, generation, or validation?
- Which nodes leave inputs, outputs, feedback, bad cases, or version records?
- Which failure boundaries should be covered by eval cases?

Choose the evaluation purpose:

| Purpose | Sample Distribution | Use Case |
|---|---|---|
| 拟合真实场景 | sample according to real task distribution | pre-release acceptance, regression, realistic user experience |
| 面向内部迭代 | evenly cover key chain nodes and likely failures | development testing, locating weak nodes, comparing versions |
| 针对边界 case | increase complex, risky, conflicting, or abnormal cases | testing refusal, fallback, permission, safety, and manual review |

Default for personal AI projects:

```text
主评测集：面向内部迭代，用来定位链路问题。
补充评测集：针对边界 case，用来证明考虑过高风险和失败边界。
如有真实记录，再补少量拟合真实场景样本。
```

Output format:

```markdown
## Step 1 / 4：评测定位

### 继承的主链路
- 项目名：
- 主链路：
- AI 介入节点：
- 输入 / 输出：
- 关键失败边界：

### 推荐评测目的
| 评测目的 | 是否采用 | 原因 | 本阶段能证明什么 |
|---|---|---|---|

### 当前不能声称什么
列出不能声称的线上效果，例如真实满意度、留存、转化、服务真实用户、准确率提升等，除非用户提供真实口径。

你认可这个评测定位吗？认可的话，我进入 Step 2，帮你确定评测对象：哪些链路节点要评，端到端用户结果要评什么；不认可的话，我们先调整评测目的。
```

## Step 2: Choose Evaluation Objects

Evaluation objects have two layers:

| Object | Purpose | Key Question |
|---|---|---|
| 链路节点评测 | locate where the system fails | 分数低了应该改 L1-L8 的哪一环 |
| 端到端评测 | judge whether the user task is solved | 用户拿到结果后是否能行动、少返工、少追问 |

### Chain-Node Evaluation

Use the Chain IDs from AIPM-Chain. Usually evaluate only the 1-3 highest-risk nodes first, not all nodes equally.

| Chain Node | Evaluation Question | Low Score Points To |
|---|---|---|
| L1 触发 | 时机、目标、触发原因是否明确 | 场景定义、触发规则、提醒条件 |
| L2 输入 | 材料是否完整、相关、可处理 | 输入字段、必填校验、补材料策略 |
| L3 预处理 | 原始输入是否被正确清洗、抽取、结构化 | 结构化规则、抽取 prompt、用户确认 |
| L4 核心处理 | 判断、检索、生成、路由是否完成节点目标 | prompt、RAG、rerank、路由、工具描述 |
| L5 输出 | 结果是否可理解、可选择、可行动 | 输出 schema、格式约束、风险提示 |
| L6 反馈 | 用户反馈是否低成本、可归因 | 反馈入口、标签体系、记录字段 |
| L7 状态沉淀 | case、memory、版本和 bad case 是否可复查 | 数据字段、状态更新、隐私清理 |
| L8 下一次/主动触发 | 历史状态是否能推动下一轮服务 | 触发阈值、频率控制、闭环设计 |

### End-To-End Evaluation

Split end-to-end signals into model-response side and user-behavior side.

Model-response side:

- 命中问题：是否回答用户真正的问题。
- 完整覆盖：是否覆盖关键条件、例外和下一步。
- 证据可信：关键结论是否有来源、理由或引用。
- 边界清楚：不确定、高风险、缺资料处是否说明。
- 行动性：用户能否基于输出继续行动。
- 表达适配：语气、长度、结构是否适合场景。

User-behavior side:

- 是否二次追问。
- 是否纠错。
- 是否明显返工。
- 是否转人工。
- 是否中断或放弃。
- 是否采纳建议。
- 是否完成任务。

If there are no real users or logs, convert behavior signals into manual labels, such as:

```text
是否仍需追问 / 是否可直接行动 / 是否需要人工大改 / 是否存在高风险误导
```

Output format:

```markdown
## Step 2 / 4：评测对象选择

### 推荐评测对象
| 评测对象 | 是否纳入第一版 | 对应链路/任务 | 为什么要评 |
|---|---|---|---|

### 关键链路节点评测
| 节点 ID | 节点名称 | 评测问题 | 低分指向 |
|---|---|---|---|

### 端到端用户结果
- 最终用户应该拿到什么：
- 模型回复端看什么：
- 用户行为端看什么：
- 没有真实用户时的替代标注：

### 本阶段不评什么
列出不纳入第一版的指标和原因。

你认可这些评测对象吗？认可的话，我进入 Step 3，帮你设计评测集、Eval case 表和 MECE rubric；不认可的话，我们先重选评测对象。
```

## Step 3: Design Eval Set And MECE Rubric

The eval set is for repeatedly testing the same chain, not for collecting impressive sample volume.

### Sample Sources

| Source | Strength | Risk | Use |
|---|---|---|---|
| 真实记录 | closest to reality | privacy, bias, permission | real distribution, regression |
| 脱敏历史材料 | grounded and defensible | must explain desensitization | personal projects, internship packaging |
| 公开材料 | safe and reproducible | may not fit the original business | portfolio demo, public prototype |
| 人工构造 case | covers frequent and risky cases | subjective | internal iteration, boundary testing |
| AI 生成初稿 | quickly expands expression variants | cannot be treated as real data | only after human screening |
| 用户模拟器 | tests multi-turn, memory, and proactive service | costly and easily unrealistic | multi-turn dialog, Agent, proactive flow |

### Sample Coverage

Use four case types:

| Case Type | Purpose |
|---|---|
| 高频常规 case | test mainstream task completion |
| 口语化 / 模糊 case | test understanding, missing-info detection, and follow-up |
| 易混淆 case | test classification, retrieval, routing, and rule stability |
| 边界 / 高风险 case | test refusal, manual confirmation, permission, and fallback |

First version sample size:

```text
20 条：跑通评测流程。
30-50 条：适合写进项目材料和面试防守。
```

### Eval Case Fields

Minimum fields:

| Field | Content |
|---|---|
| case_id | sample ID |
| case_type | 高频 / 模糊 / 易混淆 / 边界 |
| stage_id | corresponding chain node |
| input | user input or original material |
| expected | expected answer, label, or key points |
| system_output | actual system output |
| score | score based on rubric |

Advanced fields:

| Field | Content |
|---|---|
| low_score_dimension | low-scoring rubric dimension |
| failure_type | bad-case type |
| trace_note | diagnosis pointing to chain node |
| human_note | human annotation |
| version | prompt / knowledge base / flow version |
| revision_action | change made in this iteration |
| retest_score | score after retest |

### Two-Layer Rubric

Use 1-3 points by default:

| Score | Meaning |
|---|---|
| 1 | unusable, clearly wrong, risky, or not actionable |
| 2 | partially usable, directionally right but needs obvious human correction |
| 3 | usable, solves the task with only minor expression or completeness issues |

A. Business-chain diagnostic metrics:

| Dimension | Judge | Chain Link | Low Score Points To |
|---|---|---|---|
| 输入理解 | 是否识别任务、意图、槽位和约束 | L2/L3 | 输入字段、任务规格化、追问策略 |
| 上下文选择 | 是否选择相关、完整、不过载的材料 | L3/L4 | retrieval, metadata, context compression |
| 核心执行 | AI 判断、检索、生成、路由是否成功 | L4 | prompt, RAG, Agent routing, tool description |
| 输出结构 | 输出是否格式稳定、字段完整、可读 | L5 | output schema, template, format validation |
| 兜底校验 | 风险、权限、引用、人工确认是否合理 | L4/L5 | guardrail, threshold, human handoff |
| 状态回流 | 反馈和 bad case 是否被记录并影响下一次 | L6/L7/L8 | feedback fields, memory, versioning, trigger rules |

B. User end-to-end metrics:

| Dimension | Sub-Metric | Judge |
|---|---|---|
| 任务完成 | 命中问题 | 是否回答用户真正的问题 |
| 任务完成 | 完整覆盖 | 是否覆盖关键条件、例外和下一步 |
| 证据可信 | 依据清楚 | 关键结论是否有来源、理由或引用 |
| 证据可信 | 边界清楚 | 不确定和高风险内容是否标出 |
| 行动性 | 下一步明确 | 用户能否基于输出继续行动 |
| 行动性 | 可执行 | 建议是否具体到动作、对象、顺序或标准 |
| 体验风险 | 表达适配 | 输出是否符合场景语气和信息密度 |
| 体验风险 | 人工兜底 | 需要补材料或人工介入时是否及时提示 |

### Human And LLM Judging

Use this order:

```text
人工标注小样本
-> 写出人类可读 rubric
-> 人工试评 10-20 条
-> 把 rubric 转成 judge prompt
-> LLM-as-a-judge 批量初评
-> 抽检人评和机评一致性
-> 调整 judge prompt 或保留人工复核
-> 扩大评测规模
```

If LLM-as-a-judge disagrees with human labels often, use it only for pre-screening, not final claims.

Output format:

```markdown
## Step 3 / 4：评测集与评分标准

### 评测集设计
| Case 类型 | 建议数量 | 覆盖内容 | 样本来源 |
|---|---:|---|---|

### Eval case 表字段
| 字段 | 内容 | 是否第一版必填 |
|---|---|---|

### A 层：业务链路诊断 rubric
| 一级维度 | 判断问题 | 对应链路 ID | 低分指向 |
|---|---|---|---|

### B 层：用户端到端 rubric
| 一级维度 | 二级指标 | 判断问题 |
|---|---|---|

### 分值定义
| 分值 | 定义 |
|---|---|

### 人机协同评测流程
说明先人评校准，再机评预筛，再人工抽检。

你要我继续给出 Step 4 的 bad case 归因示例、复测口径和项目表达边界吗？我会明确标注这些是示例，不是已经发生的真实评测结果。
```

## Step 4: Illustrative Bad-Case Diagnosis And Retest

This step must protect evidence integrity.

Never fabricate actual bad cases, scores, user behavior, online outcomes, sample counts, or improvement percentages. If the user has not run evaluation yet, say clearly:

```text
下面是“可能出现的 bad case 示例”和“归因写法示范”，不是你项目已经发生的真实评测结果。真实报告里只能保留你实际跑样本后出现的 case、分数和修改记录。
```

### What To Provide Before Real Evaluation

Before real samples exist, provide:

1. likely failure patterns under the chosen rubric;
2. example low-score symptoms;
3. possible chain-node attribution;
4. possible modification actions;
5. retest criteria using the same samples and rubric.

Use wording such as:

```text
如果某类样本出现 X 现象，可以优先怀疑 L4，因为 Y。
这不是最终结论，需要看 trace、输入、输出和人工标注后确认。
```

Do not present examples as real project findings.

### Required Bad-Case Types

Prepare at least two illustrative categories:

| Type | Meaning | What It Proves |
|---|---|---|
| 基础链路类 | the technical or AI chain did not run correctly | can locate AI chain problems |
| 端到端体验类 | the chain technically worked, but the user task was not solved | can judge user value beyond technical correctness |

### Bad-Case Record Fields

| Field | Content |
|---|---|
| case_id | corresponding eval sample |
| bad_case_type | 基础链路类 / 端到端体验类 |
| 用户输入 | original input |
| 系统输出 | current system output |
| 低分维度 | from rubric |
| 用户侧现象 | follow-up, correction, abandonment, heavy manual edits, cannot act |
| 链路归因 | corresponding chain node ID |
| 根因判断 | why this node caused the low score |
| 优化动作 | prompt, metadata, schema, rule, fallback, or product strategy change |
| 复测结果 | score or phenomenon after rerunning the same case |

### Diagnosis Sentence Pattern

Use this pattern:

```text
这条样本在「X 维度」低分。
从 trace 看，问题可能发生在 Lx，因为 Y。
第一轮修改动作是 Z。
复测时使用同一条 case、同一套 rubric，观察 A 维度是否从 1/2 提升到 2/3，并检查是否引入新的 B 问题。
```

### Retest Criteria

Retest with:

- the same sample set;
- the same rubric;
- a named version, such as `baseline`, `v1`, `v2`;
- a recorded modification action;
- score movement by dimension;
- notes on new regressions or tradeoffs.

### Offline And Online Boundary

If there are no real users or logs, say:

```text
第一版只做离线评测。
可以写：构建样本、rubric、bad case、复测。
不能写：真实用户满意度提升、留存提升、转化提升、服务真实用户。
后续如果有真实用户，可观察二次追问率、人工修正率、采纳率和任务完成率。
```

### Safe Project Wording

Allowed wording:

```text
设计并跑通小规模离线评测，基于 __ 条样本识别出 __ 类 bad case，并将低分归因到 __ 等链路节点，提出对应优化动作，并用同一套样本和 rubric 进行复测。
```

Use blanks if the user has not run the evaluation yet.

Be careful with:

| Claim | Required Evidence |
|---|---|
| 准确率提升 XX% | sample size, baseline, comparison version, human/LLM scoring policy |
| 满意度提升 XX% | real users, scoring method, sample size |
| 转化率/留存提升 | real launch, observation period, attribution method |
| 服务 XX 用户 | real logs, user definition, privacy boundary |

## Final Output Format

When completing Step 4, output:

```markdown
## Step 4 / 4：Bad case 示例、复测口径与项目表达

### 重要说明
以下是可能出现的 bad case 示例和归因写法示范，不是已经发生的真实评测结果。真实报告里只能保留实际跑样本后出现的 case、分数和修改记录。

### 可能的 bad case 示例
| 示例类型 | 可能现象 | 低分维度 | 可能归因节点 | 示例修改动作 | 复测看什么 |
|---|---|---|---|---|---|

### Bad case 记录表
| 字段 | 内容 |
|---|---|

### 复测方案
- 样本：
- Rubric：
- 版本：
- 修改记录：
- 对比方式：
- 回归风险：

### 指标边界
- 可以写：
- 谨慎写：
- 暂时不能写：

### AI 项目评测体系卡
#### 1. 评测对象
- 项目名：
- 对应主链路：
- 评测对象类型：
- 关键链路节点：
- 端到端任务结果：
- 评测边界：

#### 2. 评测集
- 样本来源：
- 样本数量：
- 样本分布：
- Eval case 字段：

#### 3. 评分标准
- A 层链路诊断指标：
- B 层端到端指标：
- 分值定义：

#### 4. 执行方式
- 人工评分：
- LLM-as-a-judge：
- 人工抽检：
- 一致性处理：

#### 5. Bad case 与复测
- 主要低分维度：
- 主要失败类型：
- 归因节点：
- 修改动作：
- 复测口径：

#### 6. 项目表达边界
- 可以写：
- 暂时不能写：
- 后续可观察：
```

## Quality Bar

Be chain-first, evidence-conscious, and conservative.

Avoid:

- inventing real samples, scores, logs, user behavior, or improvement percentages;
- presenting illustrative bad cases as actual findings;
- evaluating all L1-L8 nodes equally when only 1-3 nodes are high risk;
- using online metrics when the user only has an offline prototype;
- treating subjective satisfaction as enough proof;
- using LLM-as-a-judge as final truth without human calibration;
- adding metrics that cannot point back to a chain node or user task;
- claiming self-evolution or proactive service improvement unless state actually changes future output or triggers.

Use conservative language:

```text
可以离线验证 / 可以构造样本 / 可以人工标注 / 可以作为示例归因 / 需要真实样本确认 / 后续可观察
```

Do not say:

```text
已经证明线上效果 / 用户满意度提升 / 准确率提升 / 自动优化 / 完整闭环 / 可规模化上线
```

unless the user provides real evidence and the evaluation design supports that claim.
