# 任务与交付契约

## 派发上下文

supervisor 为一个任务提供：

- request_id、需求版本、task_id、方案版本、attempt。
- 工作目录、隔离方式、基线标识和候选分支（若适用）。
- 目标、交付物、验收条件及其需求条目编号。
- 修改范围、非目标、相关读取依赖和独占资源。
- 已满足依赖及证据、相关项目规范与已确认决策。
- 实际验证命令或手动验收方式；未查证命令标为待确认。

缺少关键上下文时 worker 返回问题，不自行认领其他任务。无需复制整个项目历史；引用须可访问且版本可定位。

## worker 返回

返回一个结构化对象；宿主支持 JSON 时使用 JSON。必需字段：

| 字段 | 含义 |
| --- | --- |
| request_id / task_id / attempt | 与派发一致 |
| status | ready_for_review、blocked、failed 或 cancelled |
| base | 实际基线标识 |
| artifact | Git 提交、补丁路径及校验值，或只读报告位置；无成果为 null |
| changed_files | 实际改动路径 |
| verification | 实际执行的方法、结果、对应成果版本及证据 |
| limitations | 未验证内容、遗留问题、部分成果和现场 |
| question | 需要调用者回答的问题；无则 null |

返回 ready_for_review 前保存成果并停止写入该工作区。未执行的测试写明未执行，不列为通过。只读任务可交付报告，不为凑提交创建空 commit。worker 不返回任务级 done，也不等待 supervisor 合入才结束本轮。

## 账本状态

任务正常状态：pending → running → review → approved → integrating → done。

- 审核不通过进入 needs_changes；再次派发进入 running，增加 attempt。
- blocked、paused、cancelled 保存原因及可恢复阶段；取消不清除成果。
- approved 绑定 artifact 版本；成果变化后重审受影响部分。
- integrating 可表示已合入但验证或记录未完成。不能据此认为依赖已满足。
- done 必须同时具备审核记录、接纳成果证据、必要验证和已持久化账本。

每项任务只维护一个状态字段；说明和事件不得形成第二份状态表。需求总体是否满足由 supervisor 对照全部验收条件判断，包含需要人工验收的项目；不以任务数量代替验收。

## 恢复记录

在账本中按任务保存 attempt、worker 句柄（若宿主提供）、工作区、基线、候选版本、最后事件、审核结论、集成版本、下一动作。无需单建调度数据库。

恢复时核对文件、Git/补丁状态和可用句柄。超时或丢失句柄不证明 worker 已停止；状态不明时先查证，不重复派发同一任务。已集成成果核对可达提交或补丁应用证据后接续验证/记账，不重复应用。

Git 实现提交与账本记录分开：先集成实现 H，再保存记录 H 的账本检查点 L；不向 L 内回填 L 自身 SHA。账本保存失败则保持未完成，保留 H，恢复后补记。只 stage 已核对的授权文件。无 Git 以保存后的账本和可复核成果证据代替提交。
