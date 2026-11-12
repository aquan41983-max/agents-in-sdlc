---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几节练习中，你让 Copilot cloud agent 处理了三项改进用户体验和添加功能的任务。cloud agent 虽然可以自主异步工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，可以查看所有有未关闭 PR 的智能体任务、检查执行情况，并为正在进行的会话提供指导。

本课将介绍如何：

- 使用智能体页面监控 cloud agent 任务；
- 引导正在进行的会话，要求添加额外功能。

## 场景

团队要求添加高对比度模式后，发现也应增加浅色模式。由于已经在更新网站样式并添加切换功能，因此希望引导智能体同时完成两种模式。

## 查看 Copilot cloud agent 任务

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。列表应包含文档更新、修改游戏的 API，以及添加网站高对比度模式等任务。
3. 选择一个正在运行的任务，检查 Copilot 执行的操作，例如检出代码、准备环境、设置 MCP 服务器和完成任务的步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 当前的工作状态和执行方案而变化。

4. 查看右侧显示的 PR 面板，以监控 PR 和变更文件。

## 引导 cloud agent

现在让 Copilot 在实现高对比度模式时一并添加浅色模式：

1. 选择添加高对比度模式的会话。标题会因 Copilot 选择的名称和当前状态而异。

    ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到会话显示已完成设置并开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中突出显示“Steer active session while Copilot is working”对话框的屏幕截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

Copilot 会继续处理更新后的任务，并在完成当前步骤后纳入新要求。这可能需要几分钟，可以利用这段时间回顾已学内容。

## 总结和下一步

本课介绍了 Copilot agents 页面，它是监控和引导 GitHub Copilot cloud agent 任务的中心。你可以跟踪正在进行和已完成的任务、审查工作，也可以重新引导任务。

本课介绍了以下概念：

- 使用 mission control 和智能体页面监控 cloud agent 任务；
- 重新引导正在进行的会话，以添加功能。

Copilot 正在完成无障碍功能。接下来[迭代审查 Copilot 创建的 PR][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
