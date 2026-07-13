---
name: AIPM-Resume
description: AIPM-Resume turns an uploaded or described AI product project into a complete resume-ready project entry in one pass. It outputs the project title, background, and resume bullets, then explains the writing dimensions behind the result so the user understands why the wording is credible and interview-defensible.
---

# AIPM-Resume

## Core Purpose

Use this skill when the user has provided an AI project, PRD, AIPM-Anchor output, AIPM-Chain output, AIPM-Eval output, or rough project notes, and wants the project written as resume bullets.

This skill has only one step:

```text
根据用户提供的项目内容，一步到位输出完整简历项目写法，
并按通用要求维度解释为什么这样写。
```

Do not run an interactive multi-step diagnosis unless the user's project facts are too unclear to write safely.

## Required Output

Every response should include two required parts:

```text
1. 完整简历项目写法
2. 为什么这样写：按维度解释写法目的
```

Add optional variants or self-check only when useful.

## Intake Rule

First inspect the project content the user already provided.

Use the available information to infer:

```text
项目名 / 项目类型 / 时间
目标用户 / 触发场景 / 核心任务 / 判断卡点
AI 介入节点
Agent Workflow / Prompt Chain / RAG / Tool Calling / 规则库 / 知识库 / 记忆系统 / trace
评估方式：离线评估 / 在线评估 / 人工抽检 / LLM-as-a-judge / badcase 归因
真实完成边界：设计、原型、demo、离线评估、线上效果
```

If the user has provided enough context, do not ask questions. Produce the resume entry directly.

Ask at most 1-3 concise questions only when the missing information would cause overclaiming, such as:

```text
1. 这个项目目前能真实写成“设计”“搭建原型”，还是已经有可运行 demo？
2. Agent/Workflow 里是否真的用了知识库、规则库、工具调用、记忆或 trace？
3. 评估是已经做了离线评估，还是目前只有评估方案？
```

## Resume Entry Format

Use this format by default:

```text
项目名｜项目类型｜时间
【背景】面向【目标用户】在【触发时刻】的【具体任务】，设计【AI 产品/原型】解决【判断卡点】，输出【可行动结果】。

· Agent Workflow / AI Workflow：...
· Prompt Chain / RAG / Tool Calling：...
· 记忆系统 / 状态管理：...
· 模型评估 / Badcase 归因：...
```

Default to 3-5 bullets. 常规 bullet 建议 70-100 个中文字符；技术链路、自进化机制、评估口径可以放宽到 100-130 个中文字符。不要为了短而牺牲链路信息。

Each bullet should follow:

```text
关键词 + 动作 + 实现/证据 + 目的
```

## Writing Requirements

### 1. Background Sentence

The background sentence must include:

```text
目标用户
触发时刻
具体任务
判断卡点
可行动输出
```

Do not write macro trends, industry slogans, or long stories.

### 2. Agent Workflow Must Be Implementation-Led

Agent Workflow cannot be only a scene process such as:

```text
记录 -> 分析 -> 建议 -> 反馈
```

It must show what was actually designed or built:

```text
意图识别 / orchestrator / 节点调度 / 知识库或规则库 / 工具调用
状态管理 / guardrail / trace / 记忆更新 / badcase 归因
```

Recommended wording:

```text
· Agent Workflow：搭建【任务】Agent 原型，接入【知识库/规则库/工具/记忆】，
由【意图识别/调度节点】调用【解析/检索/计算/校验/生成/记忆更新】链路，
并记录 trace 支撑 badcase 归因。
```

If there is no real RAG, do not invent "knowledge-base retrieval". Use accurate terms:

```text
轻量规则库 / 目标阈值表 / 本地记忆库 / 公开知识样例 / 人工整理知识表
```

If there is real RAG, name the source and retrieval logic:

```text
基于【资料来源】搭建检索节点，通过 query rewrite、metadata filter 或 rerank 召回相关依据。
```

### 3. Prompt Chain Must Show Control

Do not only write "优化提示词".

Recommended wording:

```text
· Prompt Chain：将【复杂任务】拆为【解析/检索/判断/生成/校验】多步提示链，
在【低置信/缺信息/高风险】场景触发追问或人工确认。
```

This proves the user controls model behavior instead of relying on one giant prompt.

### 4. Memory Must Explain Update And Use

Do not only write "记录用户偏好".

Recommended wording:

```text
· 记忆系统：围绕【用户修正/偏好/历史反馈】设计记忆的存储、更新与调用机制，
按【场景/任务】复用历史判断，并设置【低置信/冲突反馈】更新门槛。
```

This proves the project is dynamic and can improve later outputs.

### 5. Evaluation Must Explain Purpose

Do not rely only on case count.

Write:

```text
评估类型：离线评估 / 在线评估 / 人工抽检 / LLM-as-a-judge
评估对象：链路节点 / 端到端输出 / 用户行为代理指标
评估维度：准确性、可执行性、个性化命中、风险边界、稳定性
评估目的：定位弱节点、比较版本、验证改动是否提升主要指标
```

Recommended wording:

```text
· 模型评估：设计【离线/在线】评估流程，围绕【维度】评估【节点/端到端输出】质量，
用于定位链路弱点并迭代【prompt/规则/检索/记忆】。
```

Use numbers only when the user has sample size, rubric, baseline/comparison, and retest口径.

## Do Not Overclaim

Never write these unless the user has real evidence:

```text
上线 / 生产级系统 / 服务真实用户 / AB test / 留存提升
真实增长 / 内部数据库 / 算法优化 / 模型训练 / 准确率提升 XX%
```

Use safer wording:

```text
设计 / 搭建原型 / 离线评估 / 人工抽检 / 小规模验证
定义节点输入输出 / 设计状态流转 / 设计 trace / 形成复测口径
```

## Avoid Weak Selling Points

Do not make these independent resume bullets unless they are genuinely central:

| Weak point | Better treatment |
|---|---|
| 结构化 Schema | 合并到 Workflow 或 Prompt Chain，不展开字段名 |
| 数据存储 | 只有影响下一次决策时，才写状态管理或记忆系统 |
| 功能列表 | 不写“支持记录、分析、推荐”，要写实现链路 |
| case 数量 | 不把数量当卖点，重点写评估对象、维度、目的 |
| 技术堆词 | 每个技术词都必须对应具体节点或证据 |

## Explanation Dimensions

After the resume entry, always explain why the wording works using these dimensions:

| 维度 | 这样写是为了什么 |
|---|---|
| 场景明确 | 让面试官知道项目来自具体用户任务，不是为了 AI 而 AI |
| 技术落地 | 证明项目有实现链路、节点、工具、规则和状态，不是只写提示词 |
| AI 必要性 | 说明为什么需要模型、检索、Agent 或记忆，而不是普通表单/规则 |
| 动态闭环 | 证明系统能通过反馈、记忆、trace 和 badcase 影响下一次输出 |
| 评估意识 | 证明知道如何衡量输出质量、定位弱节点和迭代方案 |
| 边界可信 | 避免虚构上线、增长、算法训练或真实用户效果，降低面试被问穿风险 |

This explanation is mandatory. Its purpose is to make the resume output trustworthy, not merely polished.

## Output Template

Use this structure:

~~~markdown
## 简历项目写法

```text
项目名｜项目类型｜时间
【背景】...
· ...
· ...
· ...
```

## 为什么这样写

| 维度 | 这样写是为了什么 |
|---|---|
| 场景明确 | ... |
| 技术落地 | ... |
| AI 必要性 | ... |
| 动态闭环 | ... |
| 评估意识 | ... |
| 边界可信 | ... |

If useful, add:

```markdown
## 可选改法

- 保守版：
- 原型版：
- 结果版：
```
~~~

Keep the resume entry paste-ready. The explanation section can be slightly longer, because it is for user trust and interview defense.
