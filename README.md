# Dev Progress Luna

一个轻量的，基于codex的skill集合。旨在让AI担任开发中的pm角色，按照规范的流程拆解大型任务，分配工作任务给多agent并行，并自主控制验收结果。

## 安装与入口

将 `skills/` 下的五个技能目录整体复制到宿主支持的技能发现目录，保持同级关系，例如工程的 `.agents/skills/` 或 `.codex/skills/`。宿主是否即时发现由宿主决定；文件落地不代表已激活。不要同时安装同名副本。

- `dev-progress-init`：初始化或更新项目印象及工程配置。
- `dev-progress-secretary`：澄清、确认、归档和传达本次需求。
- `dev-progress-supervisor`：制定方案，按确认策略组织实施、审核和集成。
- `dev-progress-worker`：完成一个已派发任务，交付候选成果。
- `dev-progress-shared`：公共契约，不是执行角色。

通常从初始化或秘书开始。已有确认需求可以直接恢复 supervisor，不必重走确认流程。状态查询只读。角色可以由同一会话分阶段承担，不要求创建四层 agent。

## 工程产物

默认保存在工程 `docs/dev-workflow/`，初始化时可以选择其他工程内路径。每次需求使用独立、稳定的 `REQ-日期-短标识`，恢复任务不换 ID。

```text
docs/dev-workflow/
  PROJECT.md
  config.yaml
  INDEX.md
  secretary/<request-id>/REQUEST.md
  changes/<request-id>/DEVPROGRESS.md
  changes/<request-id>/evidence/       按需创建
```

项目印象、需求、任务状态各有唯一归属。历史原地保留；索引只导航。技能升级不覆盖工程档案。

## 能力与限制

推荐 Git 隔离分支/worktree；无 Git 使用隔离副本和可验证补丁，默认串行。无子 agent 时明确切换角色串行执行；自审不声称独立审查。实际文件修改与 agent 派发需要宿主提供相应工具，本包不包含后台调度器。

默认不 push、不部署、不自动启用高级记忆。远端操作沿用用户授权及项目约定。高级扩展边界见 [扩展协议](skills/dev-progress-shared/references/extensions.md)。

本包的模板是输出资产，不是实际项目事实。初始化只生成有依据的内容；占位值不能作为确认或验收证据。
