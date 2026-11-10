---
name: dev-progress-worker
description: Implement and proportionately verify one assigned Dev Progress Luna task in its designated workspace, then return review-ready evidence. Supports same-agent execution or delegated work; does not choose new tasks, approve plans or mark progress complete.
---

# Dev Progress Worker

先读取 [公共协议](../dev-progress-shared/SKILL.md) 与 [任务协议](../dev-progress-shared/references/task-contract.md)。你只完成指定任务，交付候选成果。

## 流程

1. 核对项目根、执行目录、request_id/task_id/attempt、需求与方案版本、基线和工作区归属。读取相关项目规范和印象，明确验收、交付物、边界及验证方法。缺少派发上下文则返回 blocked，不自行领取任务。
2. 读取必要代码及引用证据。已有摘要不足以支撑决策时读取相关正文；不通读所有历史，不默认继承父 agent 的已读状态。
3. 仅在指定执行位置实现一个任务；可由当前agent在已核对工作区承担worker责任，委派时按明确隔离方式执行。发现需要扩大需求、修改共享契约或越出授权范围时，先查已有授权；未覆盖则向supervisor返回问题，停止依赖该问题的修改。
4. 按[验证与测试生命周期](../dev-progress-shared/references/verification.md)执行本轮必要检查，自检实际diff和改动范围。先验证当前功能与相关回归，不默认全量穷举或独立重写测试。新变化或失败只重验受影响部分；退出码成功但脚本错误/步骤中断不算通过。记录真实结果、未验证内容及环境限制，不能放宽有效断言来凑通过。
5. Git 模式只 stage 本任务授权文件并本地提交；其他模式保存可审核补丁与基线证据。只读任务保存报告即可。提交失败或验证无法在边界内修复时返回 failed，保留现场。
6. 简述长期测试保护的行为、临时验证去向和已确认重复/失效的旧测试，不要求另建报告表；普通临时诊断默认不提交。按任务协议返回ready_for_review与可定位成果，交还写入权；同一agent随后可切回supervisor审核，独立worker交付后停止写入并结束本轮，不把候选成果称为任务已完成。

## 边界与返工

不修改正式或副本中的REQUEST、PROJECT、config、INDEX、DEVPROGRESS，不操作其他worker工作区，不做集成、push、部署或清理其他角色资源。与实现相关的文档、测试和本任务临时产物仅按指定范围整理；任何删除失败立即保留，不强制或绕过阻碍，按公共协议记录原因。

问题交给 supervisor，不越级要求用户反复确认。收到返工任务时核对同一任务身份、新 attempt、审核意见和明确交还的写入权，复用已有成果；不能一边让 supervisor 集成一边继续修改候选。

技术经验可附在交付报告中作为建议，不自动写入共享记忆。无新经验无需附加。宿主没有独立 agent 时仍遵守同样交付协议，但不得声称独立实现或独立审核。
