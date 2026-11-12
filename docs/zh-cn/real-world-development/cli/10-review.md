---
title: "课程 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流程、两个拉取请求、可复用的自定义配置和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。本课程中，你：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到已填充的筛选 issue；
- 添加星级评分，在转发浏览器中检查结果，并手动合并第一个拉取请求（PR）；
- 从筛选 issue 开始，在 Plan 模式中确定方案、通过 Autopilot 模式实现，并在 Interactive 模式下审查；
- 使用自定义说明引导智能体，自定义现有 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查；
- 添加 Playwright Model Context Protocol (MCP) 服务器，在真实浏览器中探索筛选功能；
- 创建并选择质量保证（QA）自定义智能体，检查需求、覆盖率、技能结果和浏览器证据；
- 审查完整的筛选变更，并授权 Agent Merge 合并筛选 PR；
- 探索管理上下文、模型、共享和可选 cloud agent 委托的斜杠命令。

## 已交付的内容

本工作坊有两个 PR 阶段，每个都从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选功能、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置并包含相关测试。

从规划筛选功能到创建 PR，你一直使用同一对话和分支。为了简化工作坊，这些变更合并在一个 PR 中。

## 不同类型的验证

你通过自动化测试、浏览器手动检查和 Copilot 使用 MCP 探索浏览器等方式验证代码。`quality-checks` 技能按照新格式运行单元测试、lint 和类型检查。QA 汇总这些结果，并在 PR 前检查需求和测试覆盖率。

新增测试应解决实际覆盖缺口；如果 QA 检查后不需要新增测试，也可能是合理结果。授权合并前审查代码和证据；如果代码发生变更，应重新检查相关证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。本工作坊中，你更新了说明、自定义技能、创建 QA 配置并配置 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。说明定义标准，技能描述可重复任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。应检查实际变更和工具结果，而不只看智能体摘要。

根据任务选择**模式和模型**：用 **Plan** 先确定方案；用 **Interactive** 处理需要密切协作的改动；用 **Autopilot** 处理范围明确的任务。常规编辑可选择更快的模型，复杂工作则可选择能力更强的模型。

上下文依然与基础设施同等重要。清晰说明要构建**什么**、**为什么**以及**如何**构建，会显著影响结果。

## 进一步探索

核心工作流程已完成，还可以了解其他 CLI 功能：

- `/review`：让代码审查智能体分析变更；
- `/rubber-duck`：讨论问题并获得另一种观点；
- `/fleet`：并行协调相互独立的子任务；
- `/worktree`：隔离另一项任务；
- `/delegate`：将任务发送给 Copilot cloud agent。

## 后续步骤

熟练使用工具的最佳方式就是持续练习！可以用它处理生产代码、个人项目，或一直想做的小应用。与团队分享经验，也向团队成员学习。并且像往常一样，继续探索文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。要比较其他环境，还可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
