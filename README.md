# skill-router

farfarfun-skill 组织下所有 Skill 仓库的统一路由入口。列出每个 Skill 属于哪个仓库、解决什么问题，帮助 Codex/Claude 等编码代理在动手前先选对 Skill，再跳转到对应仓库读取具体规范。

Skill 的实现和维护仍然分散在各自的源仓库里（[`service-governance`](https://github.com/farfarfun-skill/service-governance)、[`lang-spec-hub`](https://github.com/farfarfun-skill/lang-spec-hub) 等）——本仓库不复制、不同步内容，只维护跨仓库的路由表和调用约定，避免每次新增/调整 Skill 时要在多处仓库间手工同步文档。

## 路由表

### 项目治理 —— [`project-manager`](https://github.com/farfarfun-skill/project-manager)

| Skill | 适用场景 |
| --- | --- |
| [`project-manager`](https://github.com/farfarfun-skill/project-manager/blob/master/skills/project-manager/SKILL.md) | 研发全生命周期治理：立项、设计、开发、测试、发布、复盘各阶段的产物门禁、风险与角色交接 |
| [`project-structure-governance`](https://github.com/farfarfun-skill/project-manager/blob/master/skills/project-structure-governance/SKILL.md) | 仓库规范结构：新文件该放哪、产物怎么命名、目录如何初始化与重组 |
| [`github-repo-standards`](https://github.com/farfarfun-skill/project-manager/blob/master/skills/github-repo-standards/SKILL.md) | 仓库对外呈现：默认分支命名、README 结构完整性，以及 description / Topics 这些可发现性元信息 |

### 服务工程规范 —— [`service-governance`](https://github.com/farfarfun-skill/service-governance)

| Skill | 适用场景 |
| --- | --- |
| [`service-release-governance`](https://github.com/farfarfun-skill/service-governance/blob/master/skills/service-release-governance/SKILL.md) | 约束 npm/Python 服务通过正式包发布、仓库安装和生产启动 |
| [`bash-service-guide`](https://github.com/farfarfun-skill/service-governance/blob/master/skills/bash-service-guide/SKILL.md) | 统一 Bash 服务生命周期脚本（`scripts/setup.sh`）及开发、生产运行边界 |
| [`submodule-workspace-governance`](https://github.com/farfarfun-skill/service-governance/blob/master/skills/submodule-workspace-governance/SKILL.md) | 搭建和审计以 `apps/` 下 Git 子模块聚合多个独立仓库应用的 `<product>-dev` 编排仓库 |

### 多语言开发规范 —— [`lang-spec-hub`](https://github.com/farfarfun-skill/lang-spec-hub)

| Skill | 适用场景 |
| --- | --- |
| [`python-development-standards`](https://github.com/farfarfun-skill/lang-spec-hub/blob/master/skills/python-development-standards/SKILL.md) | Python 代码、测试、项目配置、服务 CLI 契约及 farfarfun 工具复用规范 |
| [`java-development-standards`](https://github.com/farfarfun-skill/lang-spec-hub/blob/master/skills/java-development-standards/SKILL.md) | Java 代码、测试、Maven/Gradle 项目的实现和审查 |

### Paperclip 平台治理 —— [`paperclip-governance`](https://github.com/farfarfun-skill/paperclip-governance)

| Skill | 适用场景 |
| --- | --- |
| [`isolate-paperclip-work`](https://github.com/farfarfun-skill/paperclip-governance/blob/master/skills/isolate-paperclip-work/SKILL.md) | 把 Paperclip 当执行环境而非项目的一部分：隔离本地 Git 范围、命名、交付证据与清理，排除公网/云设备/发布/部署/GitHub 类工作 |
| [`paperclip-task-coordinator`](https://github.com/farfarfun-skill/paperclip-governance/blob/master/skills/paperclip-task-coordinator/SKILL.md) | 周期性 Paperclip heartbeat：拉取未完成任务、按任务/Agent 维度生成报表、对账依赖与执行状态、唤醒可执行的 assignee |

## 不含 Skill 的仓库

| 仓库 | 作用 |
| --- | --- |
| [`publish-skill`](https://github.com/farfarfun-skill/publish-skill) | 发布工具（非 Skill）：校验各仓库 `SKILL.md`、生成 `npx skills add` 安装清单、可选镜像到其他组织 |
| [`skill-router`](https://github.com/farfarfun-skill/skill-router) | 本仓库，只有这张路由表 |
| `.github` | 组织主页（`profile/README.md`） |

## 同名副本的 source of truth

`service-governance` 仓库里也有 `python-development-standards` 和 `java-development-standards`
两个目录，它们是**从 `lang-spec-hub` 同步过来的副本**，权威版本在 `lang-spec-hub`。改规范请改
`lang-spec-hub`，不要直接改 `service-governance` 里的副本。

`service-release-governance`/`bash-service-guide`/`submodule-workspace-governance` 三者互相引用密集，同放在 `service-governance` 一个仓库内以保持相对链接不失效。

## 使用

查到对应 Skill 后，在 Codex/Claude 等编码代理里直接声明要使用它：

```text
Use $project-manager to review this feature's PRD and lifecycle-stage readiness.
Use $project-structure-governance to decide where this new file belongs.
Use $github-repo-standards to audit this repo's README and Topics before publishing.
Use $service-release-governance to review how this service is packaged, installed, and started in production.
Use $bash-service-guide to design or audit scripts/setup.sh and per-service lifecycle scripts.
Use $submodule-workspace-governance to scaffold or audit this <product>-dev repository's apps/ submodules and scripts/ layout.
Use $python-development-standards to implement or review this Python change.
Use $java-development-standards to implement or review this Java change.
Use $isolate-paperclip-work to keep this Paperclip run separate from durable project assets.
Use $paperclip-task-coordinator to run the assigned Paperclip coordination heartbeat.
```

具体的安装方式（符号链接 / 目录拷贝）请参考各源仓库的 README：[`project-manager`](https://github.com/farfarfun-skill/project-manager#install)、[`service-governance`](https://github.com/farfarfun-skill/service-governance#install)、[`lang-spec-hub`](https://github.com/farfarfun-skill/lang-spec-hub#安装)、[`paperclip-governance`](https://github.com/farfarfun-skill/paperclip-governance#install)。

## 新增/调整 Skill 时

在源仓库里完成 Skill 的新增、删除或职责调整后，回到这里同步更新上面路由表的一行，保持这里是「全组织 Skill 一览」的唯一入口。同时记得更新
[组织主页](https://github.com/farfarfun-skill/.github/blob/master/profile/README.md)的仓库总览表。

## 关于组织

本仓库属于 [farfarfun-skill](https://github.com/farfarfun-skill) —— [farfarfun](https://github.com/farfarfun) 旗下的 AI Agent Skill 产品线子组织，也是全部自研 Skill 的源头。组织内各仓库分别聚焦一个治理域，把原本依赖人工评审的规范，转化为 Agent 可直接调用的确定性检查器与工作流；issue、PR 与规范变更均在此处提交。

本仓库在其中不含 Skill，只维护上面这张跨仓库路由表，是选择 Skill 的统一入口。需要批量安装（含社区 Skill）或通过 Gitee 访问，请前往镜像组织 [farfarfun-skills](https://github.com/farfarfun-skills)。

本项目基于 [MIT](LICENSE) 协议开源。
