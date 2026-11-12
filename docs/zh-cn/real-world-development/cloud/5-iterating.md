---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你让 GitHub Copilot 完成了多项改进用户体验和添加功能的任务：为代码添加文档、开发相关游戏功能，以及实现高对比度和浅色模式等无障碍功能。现在检查代码变更，必要时向 Copilot 提供反馈。

### 场景

使用生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们仍然需要审查生成的代码并遵循正常的 DevOps 流程。接下来先审查 Copilot 为文档、相关游戏功能和无障碍功能创建的变更，再向团队开放审查。

## 安全性和 GitHub Copilot cloud agent

Copilot cloud agent 异步且无人监督地执行任务，因此采取了一些安全限制：

- Copilot 对存储库只有读取权限，对用于提交代码的分支才有写入权限。
- cloud agent 在 GitHub Actions 中运行，并在独立、临时的环境中工作。
- 任何 GitHub Actions 工作流都必须由人工批准后才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个 PR：为代码添加文档。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你可以[自定义或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 在新浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Pull Requests** 选项卡并打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]**。等待 Copilot 完成工作，这可能需要几分钟。

3. 选择 **Files changed** 选项卡，检查变更。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

4. 查看新增的 TSDoc 文档注释及其他文档。具体变更可能不同。检查 TSDoc、TypeScript 规范和文件头注释；这些内容来自练习 2 中查看的自定义说明文件。
5. 确认变更合理后，返回 **Conversation** 选项卡并向下滚动。
6. 如果看到工作流等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

7. 工作流会出现在 PR 检查列表中。检查通过后，单个 Astro 应用的项目检查应全部通过；这可能需要几分钟。

## 请求 GitHub Copilot 修改

你可以像在团队成员的 PR 中一样，在评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动新会话来处理请求。结果具有非确定性，因此这里不规定固定提示；可以要求 Copilot：

- 为代码文件添加文件头注释，简要说明用途；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建 README，介绍 Astro 应用的结构。

1. 在生成的文档上添加评论，并像提及用户一样标记 **@copilot**。可以使用上面的建议，也可以提出其他文档改进要求。
2. 选择 **View Session** 观察 Copilot 如何启动新会话并修改内容。
3. 可以选择 **Back to pull request** 返回 PR。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成修改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡并审查变更。

可以继续迭代，直到满意为止。之后可将 PR 从草稿改为准备审查状态，并合并到主分支。

![将 PR 标记为准备审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来审查 Copilot 为“在游戏详情页显示相关游戏”创建的 PR：

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 选项卡，打开标题类似 **Show related games on the game details page** 的 PR。
3. 选择 **Files changed** 选项卡检查生成的代码。
4. 确认变更合理后，返回 **Conversation** 选项卡并向下滚动。
5. 如果工作流等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

6. 检查 PR 中的项目检查；全部通过可能需要几分钟。
7. **可选：**在 Codespace 中手动测试相关游戏功能。打开终端，将 `<branch-name>` 替换为 Copilot 创建的分支名称（例如 **copilot/fix-8**），然后运行：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已实现相关游戏功能。你可以像之前一样，通过在 **Conversation** 选项卡中添加评论来请求修改，例如调整显示数量，或补充文件头和 TSDoc 注释。Copilot 会据此启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。PR 应包含练习 3 中要求的高对比度模式，以及练习 4 中添加的浅色模式。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 选项卡，打开标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]**。请等待它完成。

3. 选择 **Files changed** 选项卡检查代码，重点查看：
   - 用于切换模式的 UI 组件；
   - 是否使用本地存储保存用户偏好；
   - 高对比度和浅色模式对应的 CSS 或样式变更；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 负责切换模式的 JavaScript/TypeScript 代码。
4. 确认变更合理后，返回 **Conversation** 选项卡并向下滚动。
5. 如果工作流等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

6. 检查 PR 中的项目检查。**可选：**在 Codespace 中检出 Copilot 创建的分支并手动测试：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    启动应用，在浏览器中测试高对比度和浅色模式切换，并确认刷新页面后设置仍然有效。

无障碍自定义智能体帮助 Copilot 根据无障碍最佳实践实现功能。如果发现问题，可以像之前一样在评论中标记 **@copilot**，请求更新。

## 可选练习 — 继续委托工作

cloud agent 适合处理真实待办事项，让你专注于其他工作。可以为存储库创建更多 issue 并分配给 Copilot，例如：

- 在游戏详情页创建支持者兴趣表单；
- 为游戏列表添加分页；
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 路线。本系列课程中，你：

- **查看了存储库自带的自定义说明文件**，以便稍后观察它们对 cloud agent 输出的影响；
- **将 issue 分配给 Copilot cloud agent**，观察它准备环境、规划并异步执行任务；
- **创建并使用自定义智能体**，添加高对比度和浅色模式切换；
- **使用 Copilot agents 页面作为控制中心**，监控并引导无障碍会话；
- **审查并迭代 cloud agent 的 PR**，通过标记 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 路线。如果想继续探索，其他路线可以补充本次实践：

- 🖥️ **[VS Code 路线](../../vscode/)** — 在 IDE 中使用 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 路线](../../cli/)** — 在终端中交付两项经过审查的变更，使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 功能。

在自己的存储库中，还可以尝试：

- 将重构或测试覆盖率 issue 分配给 cloud agent；
- 为其他领域创建自定义智能体；
- 为不同技术栈配置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
