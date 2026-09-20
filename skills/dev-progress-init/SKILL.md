---
name: dev-progress-init
description: Initialize or refresh a project's concise purpose, end goals and development conventions for Dev Progress Luna. Use when installing the workflow or establishing project understanding, not for implementing features.
---

# Dev Progress Init

先读取 [公共协议](../dev-progress-shared/SKILL.md)。职责是建立经过用户确认的项目印象和实际工程配置，不生成业务任务或实现。

## 流程

1. 确认工程根与文档位置。默认 workflow_root 为 `docs/dev-workflow`；用户已有约定时沿用。已有产物先读，不覆盖、不重新初始化。
2. 读取项目指令、README、贡献/格式规范和验证配置，按需抽样现有风格。规范有冲突时记录差异，不从个别文件推导全项目规则。
3. 对项目用途、目标用户、最终形态、成功标准、长期体验取舍中仍不明确的内容，向用户澄清。环境能回答的技术问题自行查证；问题数量按实际缺口控制。空工程不虚构现有技术栈。
4. 用 [项目印象模板](assets/PROJECT.md.template) 提出一屏内的草案，取得用户确认或记录已有明确确认。需要等待时可保存标为 draft 的草稿，不能当作正式规范。
5. 用 [配置模板](assets/config.yaml.template) 保存已查证信息；只创建 PROJECT.md、config.yaml 和简短 INDEX.md 导航。配置不明保持 null 或 unknown，不猜命令。此时不创建空需求、空进度或知识目录。
6. 报告文件、印象确认状态与实际能力限制。用户已要求进入需求流程时，带着原请求转入 secretary；否则结束初始化。

## 印象边界

只记录用途、最终目标、体验取舍、代码风格、协作规范和长期边界。不记录类/函数名、模块清单、接口字段、目录架构、当前 bug、任务进度或命令。具体规范引用原文，避免复制大段细节。

更新只处理已有信息的缺口或明确变化，展示差异并记录确认依据。印象是用户意图与项目约定的精简锚点，不覆盖项目指令。技能更新不自动改写工程印象。

## 配置与兼容

配置使用简单 YAML 映射和列表，工具可手动读取，不要求专门运行库。路径相对工程根，引用解析后核对实际范围。检测到 Git 不代表获准 push；无 Git 不自动 git init。模型只记用户指定的选择；秘书优先由宿主可用且用户允许的高推理能力模型承担，不硬编码模型名称或声称已切换模型。

配置字段含义：project_rules 是工程内规范路径列表；verification 每项记录 method（命令或人工方法）、when（适用变更）及 source（查证来源）。mode 为 serial 或 auto，auto 仅在独立性成立时并行；max_workers 是上限。isolation 为 auto、git 或 copy；auto 优先 Git，否则隔离副本。target_ref 为用户或项目确定的最终目标，null 表示尚未选择，不能猜为 main。capabilities 使用 true、false 或 unknown，执行前查证。remote_sync 记录 not_requested 或实际授权说明，不因配置存在自动授予权限。extensions 只预留开关，启用前必须有具体能力及写入授权。
