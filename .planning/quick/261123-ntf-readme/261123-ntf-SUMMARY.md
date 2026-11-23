---
phase: quick-261123-ntf
plan: 01
status: complete
subsystem: docs
tags: [architecture, drawio, go, docker, sing-box]
requires: []
provides:
  - 固定源码对应的中文逻辑架构与核心运行图
  - 当前单容器实现、内嵌模式与历史内容的边界说明
affects: [architecture, documentation]
tech-stack:
  added: []
  patterns: [源码与预览同步维护]
key-files:
  created:
    - docs/diagrams/cloud-cli-proxy-architecture.drawio
    - docs/diagrams/cloud-cli-proxy-logical.png
    - docs/diagrams/cloud-cli-proxy-runtime.png
  modified:
    - docs/zh/guide/architecture.md
    - README.md
key-decisions:
  - 以当前 Linux 单容器 provider 与 Compose embedded 为主视图。
  - 既有指南完整保留在历史段，当前源码解释置前。
  - 仅交付文档分支，避免 main 文档变动触发 Pages 部署。
requirements-completed: []
completed: 2026-11-23
---

# Quick 261123-ntf：当前源码架构文档

完成固定提交 b794026f7fafc737a9cd8bb26eb07aa81f8ab952 的两页可编辑架构图、PNG 预览、指南和 README 导航，区分当前单容器网络与历史旁车说明。

## 交付内容

- 逻辑图覆盖浏览器和 CLI、控制面职责、内嵌 Worker、用户容器及状态存储。
- 核心运行图选择 Compose embedded 模式；控制面 API、SSH Proxy、周期任务与 Worker 没有误画成多个常驻服务。
- 指南说明 entry 不自动启动非 running 主机、socket 实现缺少当前独立命令入口、旧白名单未接入默认新配置，以及只读配置挂载与入口删除动作的装配冲突。
- 既有架构指南全文保留，README 的既有说明保留；图源和预览有相互导航。

## GSD 执行记录

使用官方 GSD SDK 的 `query init.quick` 创建任务上下文，确认 ROADMAP 和 planning 存在。`frontmatter.validate` 与 `verify.plan-structure` 均通过；读取了仓内 planner、executor、quick 工作流和状态文件。既有协作执行角色依照这些本地合同完成任务，没有使用 SDK run/auto 发起外部模型请求，也没有修改 CLAUDE 规则或工作流配置。

本地隔离分支先保存 PLAN，再建立 `worktree-agent-cloud-architecture` 执行分支。下面是本地执行提交记录；远端文档提交由统一发布流程另行生成，不能将这些本地哈希当作线上分支回执。

| 内容 | 本地提交 |
| --- | --- |
| 预提交计划 | `1ab7954dde2d8f04f6fb594f525f04b2cbf0ec4c` |
| 任务 1、2 的完整架构文档集合 | `1f00157164aedc077b8ca1b46afec28906691106` |

## 检查结果

- 原始抓取文件的 Git blob 哈希与固定基线逐一一致。
- 指南原文完整保留；新增与保留的 Markdown 文件链接目标均存在。
- draw.io XML 结构正确，每页 ID 唯一且包含所需图层。
- 最终两页本地渲染均无文字溢出、标签碰撞或连线穿节点；实际检查两张最终 PNG。
- `git diff --check` 通过；提交仅包含文档和图形，没有业务代码、运行配置或规则变更，也没有文件删除。
- 新增文稿和规划产物检查未引入本机绝对路径或真实凭据。

原生 draw.io 编辑器完整导出未验证；没有启动应用、Docker、真实数据库或代理，也没有执行部署及网络隔离验收。

## 与计划的差异

原 README 有两处指向 `LICENSE` 的链接，但固定源码树没有该文件。将其最小改为不带链接的徽章与 MIT 文字，并注明基线缺少文件；没有新增或推定许可证正文。

第一次本地任务提交碰到仍在结束的暂存进程所持有的 index 锁。等待该进程完成后，两个相关文档任务共同保存在一个原子提交中；未清除活动锁、强制回滚或重写已有历史。规划与执行范围保持不变。

## 后续边界

本任务不推进 ROADMAP 阶段，不改变 v4.0 发布状态。网络运行限制已记录于指南，后续运行修复及验收应另建任务。文档分支可审阅；main 的 Pages 自动发布未触发。

## Self-Check: PASSED

五个交付文件、计划文件和本总结存在；上述两个本地提交可以在执行仓库历史中找到；图文与检查结果一致。
