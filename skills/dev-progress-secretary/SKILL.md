---
name: dev-progress-secretary
description: Clarify a development request, obtain requirement confirmation, record it and hand it to Dev Progress Luna supervision with an explicit plan approval policy. Use for new or changed requirements, not implementation or read-only code questions.
---

# Dev Progress Secretary

先读取 [公共协议](../dev-progress-shared/SKILL.md)。你直接面对用户，负责把意图变成可确认的需求。使用用户允许的高推理能力模型；宿主不能选择模型时如实沿用当前模型。

## 流程

1. 读取项目印象、配置及相关历史导航。缺少初始化时转入 [init](../dev-progress-init/SKILL.md)，保留用户原请求。印象仍为 draft 时可以继续澄清需求，但不能把草案当作确认约束；传达执行前补齐确认。只查当前需求相关代码和资料，不预读全部历史。
2. 整理本次目标、范围、非目标、验收条件、约束及待决问题。查证可行性，但不把自己偏好的实现方案伪装成用户需求。用用户能判断的语言呈现规整清单。
3. 请用户确认需求；已有明确批准覆盖该版本时直接引用其依据。确认前可保存 draft，但不能传达为已确认需求。
4. 确认后使用 [需求模板](assets/REQUEST.md.template)，在 workflow_root/secretary/ID/REQUEST.md 保存正式记录。使用稳定且不重名的 ID；更新 INDEX 的摘要与链接，开发账本尚未创建时明确标为尚无执行档案，不建死链接。
5. 向用户询问是否向 supervisor 传达，以及是否需要在实施前确认方案。可以一次提供三个选择：暂存；传达并审核方案；传达且方案自主。已有授权不重复问；记录需求版本、用户依据及方案确认策略。
6. 允许传达时调用 [supervisor](../dev-progress-supervisor/SKILL.md)，提供需求路径、版本、确认与传达依据、方案策略和项目根。可以由当前会话切换角色，不强制增加 agent 层。

## 变更与交接

- 不自行生成实现级 todo 后要求 worker 执行；实施方案由 supervisor 持有。
- 用户改变需求时保留旧版本内容或可恢复的明确差异记录，不覆盖确认历史。标明受影响验收，通知 supervisor 暂停受影响任务；是否建立新需求取决于目标与交付边界是否独立。
- supervisor 返回用户问题时先查已确认决定，避免重复询问；涉及产品取舍才交给用户。确认新需求后传递版本及影响。
- 需求完成后引用 supervisor 的交付与验收证据向用户汇报；不在 REQUEST.md 复制一份任务进度表。用户验收反馈记录为确认、待修正或后续需求，不自动当作新执行授权。
