---
name: aipm-resume
description: Turn an uploaded or described AI product project, PRD, AIPM-Anchor, AIPM-Chain, AIPM-Eval output, or project notes into a credible, interview-defensible resume project entry. Use when a user needs to package an AI product project for a resume, explain its contribution clearly, or avoid overclaiming implementation and results.
---

# AIPM-Resume

## Core Task

Write an AI project as a small number of verifiable contribution units, not as a feature list or a fixed set of technical labels.

Use this reasoning chain:

```text
real user task
-> key product or system decision
-> implementation chain
-> validation evidence and iteration
```

Not every project needs a separate bullet for every link. Use the chain to decide whether the project expression is complete and which evidence deserves space.

## Build an Evidence Model First

Before drafting, turn source material into verifiable relationships:

```text
problem -> decision -> action -> evidence
```

Identify the project's user task, the decisions the user actually made, the mechanism or chain that realizes each decision, and the evidence that supports the claim. Do not begin by searching for technical keywords or a resume template.

Classify each claim by its completed boundary:

- `设计完成`: the user defined a solution, rule, workflow, prototype specification, or evaluation plan.
- `原型/实现完成`: the user built or configured a runnable artifact.
- `离线验证完成`: the user ran a defined evaluation, comparison, or review process.
- `线上验证完成`: the user has real usage, behavior, or business evidence.
- `未提供`: the material does not support a claim.

Use verbs and result language that match the boundary. Never turn a plan into an implementation, an offline result into user behavior, or a simulated result into an online outcome.

Ask at most three concise questions only when a missing fact would materially change the claim boundary or make the resume misleading. Otherwise, draft from the available evidence and mark the limitation.

## Select What Belongs in the Resume

Prioritize the strongest `decision -> action -> evidence` relationships. A good AI project entry normally lets a reader understand:

1. What real task the project addresses.
2. What consequential decisions the user made.
3. How AI, rules, tools, people, or state work together at the relevant point in the chain.
4. How quality is judged and how that judgment affects iteration.

Do not force all four into separate bullets. Omit weak or unsupported material; combine related details when they prove the same contribution.

## Write Contribution Units

Use a concise project-background sentence to establish the user, task, and intended outcome. For a standalone entry, `项目背景：` is useful when it improves scanning.

Write each bullet as one complete contribution unit:

```text
action -> method or mechanism -> purpose, evidence, or result
```

Derive the bullet's opening phrase from its primary action. First identify the decision or change the user owned, then compress that action into a short verb-led phrase. Do not select a title from a preset list of technologies.

The heading should answer "what was done"; the rest of the bullet should answer "why, how, and with what evidence." Use a technical term only when it explains a real mechanism, dependency, control point, or boundary in the project.

Do not mention an Agent, RAG, Skill, MCP, memory, trace, or workflow merely to signal sophistication. Name it only if the material shows what role it played and how it changed the task chain.

## Express Evaluation as a Decision Loop

Do not treat evaluation as a generic final step or a case-count claim. When evidence exists, express it as:

```text
observed phenomenon
-> selected metric and why it represents the phenomenon
-> decision or chain change informed by the metric
-> comparable result
```

Use a numeric result only when the material supplies the metric definition, measurement scope, comparison basis, and score source. State whether the evidence is online, offline, human-reviewed, or simulated. If the project only defines an evaluation approach, write the metric's diagnostic purpose and mark the result as `【待补充】`.

## Guardrails

- Do not describe features without showing the decision or mechanism that makes them meaningful.
- Do not use a fixed list of bullets, technical labels, verbs, or explanation dimensions as a template to fill.
- Do not write results, users, launches, experiments, system capabilities, or technical components absent from the source material.
- Do not treat a model's generated content as a verified external fact unless the project uses a stated source or tool to verify it.
- Keep each bullet focused on one primary contribution. Prefer omission over a dense, untraceable sentence.

## Output

Return:

1. A paste-ready project title and concise project-background sentence.
2. The smallest useful set of resume bullets, each built from a strong contribution unit.
3. A short `事实与边界说明` that maps each bullet to its source evidence and flags `【待补充】` claims.

Offer alternative phrasing only when the project has evidence at materially different completion boundaries, such as a design version and an implemented version. Keep the main output concise and interview-defensible.
