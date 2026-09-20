---
name: dev-progress-shared
description: Shared contracts for Dev Progress Luna roles, including ownership, authorization, task identity, evidence and handoffs. Load as a prerequisite when using a Luna role; does not initiate development.
---

# Dev Progress Shared

本技能是公共协议。读取它不触发初始化、派发或实现。

## 路径与能力

- skill 相对引用以当前文件所在目录解析；工程产物以明确的项目根与 workflow_root 解析。不能从技能安装目录猜项目根。
- 使用宿主真实可用的文件、提问、agent 和版本控制能力。不依赖具体 API 名称，不把配置声明当作能力验证。
- 首次进入读取项目指令、项目印象及配置；随后只读取当前需求、当前账本与相关依据。缺失文件交回对应所有者补齐；只读查询不得顺带初始化。
- 状态查询没有明确需求 ID 时先查 INDEX；存在多个候选则澄清，不把最新日期当作当前授权。

## 信息与写入权

| 产物 | 内容 | 唯一写入角色 |
| --- | --- | --- |
| PROJECT.md | 用途、终局目标、风格、长期规范 | init，变更须有用户依据 |
| config.yaml | 工程路径、验证方式、能力和执行约定 | init；执行中配置变更须明确记录依据 |
| secretary/ID/REQUEST.md | 需求版本、验收、确认与传达 | secretary |
| changes/ID/DEVPROGRESS.md | 方案、任务状态、审核、集成与恢复证据 | supervisor |
| INDEX.md | ID、摘要、档案链接 | secretary；可从档案重建，不存独立任务状态 |
| 候选实现及交付报告 | 授权任务的成果与验证 | 指定 worker |

同一 agent 可以明确切换角色，但不能因此绕过确认或完成门槛。worker 不写任何正式账本副本，也不修改需求或印象。问题沿 worker → supervisor → secretary/用户返回；能自行查证的问题先查证，不重复问已确认事项。

## 授权与版本

分别记录需求确认、是否传达、方案是否需要确认。确认绑定准确版本及用户消息依据；明确用户指令可以一次覆盖多项，不重复索取已有授权。沉默、等待超时和预选项不是同意。

需求确认不自动等于授权执行；免方案确认允许 supervisor 在已确认范围内作实现决策，不允许改变目标、验收或扩大外部操作权限。方案需要确认时可以调查和编写草案，不能开始实现。

需求变化由 secretary 记录版本和差异；supervisor 暂停受影响任务，核对已有成果、依赖和新确认范围。纯实现顺序调整不机械重走全部确认；改变已批准的重要设计取舍时确认受影响部分。

## 交接与状态

派发、恢复和审核时读取 [任务协议](references/task-contract.md)。任务身份为 request_id + task_id，尝试编号独立计数，重试或换 agent 不换任务身份。

worker 交付就绪只意味着待审核；只有 supervisor 在审核、集成及必要验证完成后标记完成。证据绑定具体版本，不能只凭自然语言的“完成”推进依赖。

所有持久化文件使用 UTF-8。文件写入、提交或集成失败时保留现场，记录实际达到的状态；禁止把失败写成完成。不得清除用户改动以制造干净基线。

## 扩展

仅在接入可选记忆或高级调度时读取 [扩展协议](references/extensions.md)。核心流程不要求向量库、MCP、动态 skill、固定编程运行时或 worktree 池。
