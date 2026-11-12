---
title: "课程 8 - 创建并合并功能 PR"
description: "一起审查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能、说明更新、技能更新、质量保证（QA）配置和测试都保存在同一分支上。现在一起审查这些变更并创建拉取请求。本次不再像星级评分 PR 一样手动合并，而是使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 实际项目通常会将功能、说明、技能和 QA 智能体拆分到多个 PR 中。为简化本工作坊，我们让完整的筛选和质量工作流共用一个对话和分支，并放入同一个 PR。

本课将介绍如何：

- 了解 Agent Merge 如何自动化合并流程；
- 检查完整的功能变更和验证证据；
- 创建筛选功能 PR；
- 审查后启用 Agent Merge 并确认 PR 已合并。

## 场景

在筛选工作流中，你使用 Copilot 规划、实现并验证了一项功能。Tailspin Toys 现在希望自动处理剩余 PR 工作，同时由开发者掌控合并授权。

## 介绍 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会解决 PR 阻塞项，例如修复持续集成（CI）检查失败、处理审查意见，以及在需要时变基；然后在存储库允许的情况下启用 GitHub 自动合并。

此前你是手动选择 **Merge pull request**。Agent Merge 可以代为处理，但启用前仍须先审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码完成后，一起审查代码、创建 PR，并让 Agent Merge 处理其余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default** 并按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。检查全部通过后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限限制。如果合并受阻，请查看原因，并在存储库允许时手动完成已审查的合并。

## 总结和下一步

你已自动化开发流程中的多个步骤，包括生成代码、测试和验证代码，以及管理 PR。本课中，你了解了 Agent Merge，检查完整功能和验证证据，创建筛选功能 PR，并在审查后启用 Agent Merge 确认合并。

接下来[探索更多 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享和可选的 cloud agent 委托。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
