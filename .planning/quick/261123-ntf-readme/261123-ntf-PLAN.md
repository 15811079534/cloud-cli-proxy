---
phase: quick-261123-ntf
plan: 01
type: execute
wave: 1
depends_on: []
autonomous: true
files_modified:
  - docs/zh/guide/architecture.md
  - README.md
  - docs/diagrams/cloud-cli-proxy-architecture.drawio
  - docs/diagrams/cloud-cli-proxy-logical.png
  - docs/diagrams/cloud-cli-proxy-runtime.png
must_haves:
  truths:
    - 当前视图可从 README 和既有架构指南进入。
    - 每条主要运行关系有固定提交源码依据。
    - 旧网关说明与当前同容器实现、独立代理与内嵌模式有明确界限。
  artifacts:
    - path: docs/zh/guide/architecture.md
      provides: 当前源码视图、关系说明与证据索引
    - path: docs/diagrams/cloud-cli-proxy-architecture.drawio
      provides: 可编辑的逻辑和运行两页源图
    - path: docs/diagrams/cloud-cli-proxy-logical.png
      provides: 逻辑架构预览
    - path: docs/diagrams/cloud-cli-proxy-runtime.png
      provides: 核心运行预览
  key_links:
    - from: README.md
      to: docs/zh/guide/architecture.md
      via: 架构导航
    - from: docs/zh/guide/architecture.md
      to: docs/diagrams/cloud-cli-proxy-architecture.drawio
      via: 编辑源图及预览链接
---

# 补充当前源码架构图

<objective>
为 Cloud CLI Proxy 补充中文逻辑分层和核心运行图，帮助读者区分当前代码、部署示例和历史设计。固定基线为 b794026f7fafc737a9cd8bb26eb07aa81f8ab952。用户已授权架构文档提交；应用部署不属于本任务。
</objective>

<context>
@CLAUDE.md
@.planning/STATE.md
@README.md
@docs/zh/guide/architecture.md
@internal/controlplane/app/app.go
@internal/controlplane/http/router.go
@internal/runtime/runtime_service.go
@internal/runtime/tasks/worker.go
@internal/network/container_proxy_provider.go
@deploy/docker/managed-user/entrypoint.sh
@docker-compose.yml
</context>

<tasks>
<task type="auto">
  <name>任务 1：核对当前执行路径并扩充既有指南</name>
  <files>docs/zh/guide/architecture.md</files>
  <action>读取本公开仓的入口、路由、任务编排、网络提供器、容器入口、SSH 代理、前端 API 及部署工作流。保留原指南全文，在开头增加当前源码章节，列出逻辑职责、实际进程边界、主链路和源码链接；说明旧旁车模型与当前单容器路径、代理模式差异和未经运行验收的范围。不得引用其他仓库内部实现、真实凭据或本机绝对路径。</action>
  <verify><automated>git diff --check</automated>核对指南原字节保留、当前断言逐条回到固定源码；最终图文统一后执行链接检查。</verify>
  <done>指南保留原文，新增当前视图与可追溯证据，未把规划、注释或依赖声明当作已运行行为。</done>
</task>
<task type="auto">
  <name>任务 2：生成两页源图和预览并接入导航</name>
  <files>README.md, docs/diagrams/cloud-cli-proxy-architecture.drawio, docs/diagrams/cloud-cli-proxy-logical.png, docs/diagrams/cloud-cli-proxy-runtime.png</files>
  <action>按用户已接受的图形格式，在本地生成可编辑两页 draw.io 和 PNG；逻辑层按真实职责归纳，运行图选择单一明确模式并在正文列出替代模式与省略的次要链路。README 保留原文并加入指南、源图和预览。完成图文一致、XML、链接、文字溢出和连线碰撞检查及实际图片审阅；记录原生 draw.io 完整导出尚未验证。远端仅交付文档分支，避免 main 的 docs.yml 自动部署。</action>
  <verify><automated>git diff --check</automated>外部本地渲染验证报告必须无文字溢出、标签碰撞、连线穿节点；两张最终 PNG 逐图检查，源图 XML 与所有新增链接校验。</verify>
  <done>五个交付文件齐全，导航有效，两页图片清晰，源码和预览一致；没有业务代码、部署配置或规则变更。</done>
</task>
</tasks>

## 来源覆盖核对

| 来源 | 本任务适用内容 | 覆盖 |
| --- | --- | --- |
| GOAL | 用户要求给现有项目绘制架构并提交 GitHub | 任务 1、2 |
| REQ | 本任务为独立 quick 文档任务，无 ROADMAP 阶段需求指派 | 不修改路线图 |
| RESEARCH | 直接读取固定源码；不增加技术选型或外部模型调用 | 任务 1 |
| CONTEXT | 中文、保留原文、只用本公开仓证据、本地绘图、仅文档分支 | 任务 1、2 |

<threat_model>
本次只修改文档和图形，没有新增应用信任边界。公开文档的信息披露风险通过仅使用本仓公开代码、排除真实配置与会话数据、扫描新增本机绝对路径控制。main 上 docs 路径会触发 Pages 部署，因此交付独立文档分支。
</threat_model>

<verification>
核对固定基线与源码 blob；执行文档链接、XML 和本地渲染检查；人工查看最终两张图；检查提交白名单和新增敏感模式。保留本地任务提交记录，生成 SUMMARY 并在 STATE 追加 quick 完成记录。GSD SDK 仅执行本地 query 合同与状态命令；现有协作执行角色按仓内 planner/executor 文件完成任务。
</verification>

<success_criteria>
指南和图完整、可读、可编辑、可追溯；原文保留；实际代码、示例部署和历史设计边界明确；本任务没有部署应用或执行网络隔离验收。
</success_criteria>

<output>
.planning/quick/261123-ntf-readme/261123-ntf-SUMMARY.md；STATE quick 完成记录；五个架构交付文件。
</output>
