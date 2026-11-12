---
title: "课程 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

编写代码只是开发工作的一部分。我们还要处理数据、外部服务，并让 Copilot 使用更多自动化工具。MCP 服务器可以扩展 Copilot CLI 内置的功能，为它提供其他工具和服务。

本课将介绍如何：

- 了解 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它；
- 如果尚未配置，则添加 Playwright MCP 服务器；
- 让智能体操作浏览器并探索筛选功能。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体提供与外部工具和服务通信的方式，使它们能够访问最新信息并代表你执行操作。

AI 智能体通过 MCP 服务器访问工具和资源。MCP 服务器负责连接智能体与外部工具和服务，每个服务器代表一组可供智能体使用的工具和资源。常见服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、issue 和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，方便发现和参与 MCP 生态系统。

> [!CAUTION]
> 对待 MCP 服务器应像对待项目中的其他依赖项一样。使用前检查源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Playwright MCP 服务器尚未配置，请将其添加，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已经配置了 Playwright MCP，请跳过设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 将名称设为 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中输入：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前你已手动确认功能正常。现在使用 Copilot 中的 Playwright MCP 服务器重复检查：

1. 输入以下提示，让 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   启动应用，并使用 Playwright MCP 对照 issue 和计划检查筛选功能。告诉我哪些有效、哪些无效，不要进行任何更改。完成后停止你启动的服务器。
   ```

> [!NOTE]
> 通常 Copilot 会自动找到并使用 MCP 服务器，不一定需要在提示中指定。不过明确指出工具有助于保持结果一致，也能减少一些令牌消耗。

2. 观察 Copilot 如何在浏览器中执行检查步骤。
3. 阅读报告并确认行为符合预期。

Copilot 会启动服务器、使用 Playwright 浏览网站、停止服务器并汇报结果。

## 总结和下一步

恭喜，你已在真实浏览器中使用 Playwright MCP 探索功能。本课中，你了解了 MCP 和 Copilot CLI 的用法，在尚未配置时添加 Playwright MCP，并让智能体操作浏览器检查筛选功能。

接下来创建一个[结合技能和浏览器工具的 QA 自定义智能体][next-lesson]。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
