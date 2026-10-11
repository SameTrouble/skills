---
name: subagents-router
description: 在任务需要委派子代理时使用；用户尚未指定策略的话，先询问采用 adaptive-subagents 还是 luna-subagents，再加载所选策略。
---

# 子代理策略路由

这是需要委派子代理时的自动入口。先确认本任务采用哪种策略，再加载相应的策略 skill。

## 选择策略

- 如果用户在当前任务中已明确点名 `adaptive-subagents` 或 `luna-subagents`，将其视为策略选择，不重复询问。
- 否则，在启动任何子代理前，向用户询问本任务使用哪种策略，并清楚列出两项：
  1. **自适应策略（`adaptive-subagents`）**：依照任务类型与难度，使用 `gpt-6-luna` + `max` 或继承主会话的模型与思考强度。
  2. **Luna 固定策略（`luna-subagents`）**：常规子代理使用 `gpt-6-luna` + `max`；代码审查继承主会话的模型与思考强度。
- 每个任务只询问一次；用户选定后，本任务的后续子代理都沿用该策略，除非用户要求切换。
- 用户尚未选择时，不启动子代理。等待答复期间，主代理可以继续不依赖子代理的工作。

## 加载所选策略

选择确定后，使用 Skill 工具显式调用对应的 skill：

- 自适应策略：`adaptive-subagents`
- Luna 固定策略：`luna-subagents`

遵循已加载策略的调度规则。不要在策略尚未确定时自行选择、混用或替用户决定。
