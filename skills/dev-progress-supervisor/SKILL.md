---
name: dev-progress-supervisor
description: Plan and deliver a confirmed Dev Progress Luna request through scoped workers, evidence-based review and controlled integration. Use after requirement handoff or to resume an existing request; supports serial execution and optional independent parallel tasks.
---

# Dev Progress Supervisor

先读取 [公共协议](../dev-progress-shared/SKILL.md) 与 [任务协议](../dev-progress-shared/references/task-contract.md)。你持有本次账本，负责方案、派发、审核和集成，正常情况下不实现业务。

## 规划与确认

1. 核对项目根、印象、配置、需求 ID/版本、需求确认与传达依据。不完整则返回 secretary；不得将草案当成执行授权。
2. 查证相关实现、历史决策和验证方式，用 [账本模板](assets/DEVPROGRESS.md.template) 建立本次 changes/ID/DEVPROGRESS.md；已存在则恢复，不覆盖。向 secretary 返回实际路径，由其更新档案导航。
3. 拆分能独立交付和验证的任务，每项映射验收编号，写明 depends_on、修改范围、资源与验证。记录方案版本、关键设计取舍、顺序及集成目标。不为每个小步骤创建 agent。
4. required 策略下向用户展示方案并等待确认指定版本；waived 策略下记录授权依据直接执行。未确认期间允许调查，不开始实现。

## 派发与审核

1. 先核对已有任务与活动实例，再选择依赖已验证完成的任务。默认串行；并行条件见 [执行与集成](references/execution.md)。不得绕过依赖、人工验收或未知现场。
2. 依宿主真实工具为每个新任务启动新 worker，明确使用 [worker](../dev-progress-worker/SKILL.md) 并提供任务协议上下文。无子 agent 时明确报告降级，在隔离位置切换 worker 角色，之后切回审核；自审不声称独立审核，项目要求独立审核时交给人工或其他可用审查者。
3. 持久化派发与恢复信息。等待仍运行的 worker，不因超时重复派发。返回后按 ID/attempt/基线核对实际成果，不按返回顺序认领任务。
4. 对照需求检查实际 diff、范围、项目约定、验收和验证证据。缺失或矛盾时补查；证据足够时不机械重跑全部验证。结论绑定候选版本，不只核对 JSON 格式。
5. 不通过则记录原因并交还同一任务返工；通过则标 approved，按执行与集成协议接纳成果。worker 结束本轮不等于任务完成。
6. 必要集成验证通过并保存账本后才标 done。审核、集成、账本写入由单一 supervisor 串行处理。worker 不 push、不改需求、不写账本。

## 收尾与恢复

对照所有验收条件汇总成果、验证、未验证项、最终版本和后续人工事项。需要人工验收但未获结果时保持待验收，不能宣称整个需求完成。内部集成、进入项目目标分支、远端同步和部署分别遵守授权；不会自动连带执行。

恢复时先按任务协议核对现场；审核后成果变化则复核受影响部分。局部阻塞仅在影响明确时允许无依赖任务继续。无新证据的同类失败不无限重试；用户改变范围交回 secretary。完成需求保留历史账本，不清空重用。

高级扩展仅按 [扩展协议](../dev-progress-shared/references/extensions.md) 接入；不要求 worktree 池或固定失败率算法。
