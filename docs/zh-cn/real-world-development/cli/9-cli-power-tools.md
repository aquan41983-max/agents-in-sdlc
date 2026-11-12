---
title: "课程 9 - GitHub Copilot CLI 中的斜杠命令"
description: "探索管理上下文、选择模型、共享会话和可选委托到 cloud agent 的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他 CLI 工具一样，GitHub Copilot CLI 提供许多斜杠命令，以使用高级功能、查看内部信息并进行其他配置。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr`。本课继续介绍一些实用命令。

本课将介绍如何：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文；
- 使用 `/model` 浏览可用模型；
- 了解 `/share` 如何导出或共享会话；
- 探索用于并行工作、工作树和委托给 cloud agent 的可选命令。

## 场景

核心 CLI 工作流程已经完成。现在了解其他功能，包括管理上下文、切换模型、共享会话，以及可选地将工作委托给 [Copilot cloud agent][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会接近模型上下文窗口的上限。Copilot CLI 会在需要时自动压缩对话，也可以使用斜杠命令检查或手动压缩。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前令牌用量，以及上下文在系统说明、工具、消息和可用空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。想自行决定压缩时间时，可以使用 `/compact`。切换到无关任务并需要新对话时，使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发者的偏好也不同。Copilot CLI 可以列出并选择模型：

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作和分享经验有助于提升所有人使用 AI 工具的能力。`/share` 命令可将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help` 并查看当前版本中的 `/share` 选项。
2. 如需共享会话，选择合适的目标，例如 `/share file`，导出到本地 Markdown 文件。
3. 发送给他人或发布前，先审查导出内容。会话导出可能包含提示、回复和项目细节。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库或会话内容。

## 可选：扩展并行工作或委托任务

核心工作坊已结束。Copilot CLI 还为大型任务提供以下命令：

- `/fleet`：将相互独立的子任务分配给多个子智能体并行运行。
- `/worktree`：为单独任务创建隔离的 Git 工作树。
- `/delegate`：将任务发送给异步工作的 Copilot cloud agent；它可能会创建拉取请求。

这些命令是可选的，因为它们可能创建额外工作树或触发远程工作。试用前先创建一个范围清晰的新任务，并按正常流程审查结果。想进一步了解异步智能体工作，可继续学习 [cloud agent 工作坊][cloud-workshop]。

## 总结和下一步

使用 Copilot CLI 斜杠命令可以进行配置、共享会话，并了解后台运行情况。本课中，你使用 `/context` 和 `/compact` 探索上下文管理，使用 `/model` 查看模型，了解如何用 `/share` 导出会话，并探索用于并行工作、工作树和委托给 cloud agent 的可选命令。

Copilot CLI 还有更多命令和功能！接下来[回顾所学内容][next-lesson]，并了解后续学习方式。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
