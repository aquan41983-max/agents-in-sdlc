---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚使用智能体模式构建了筛选功能。创建拉取请求前，应先在浏览器中确认功能正常。现在连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器来测试功能，然后发布分支并创建 PR。

本练习将介绍如何：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何扩展 Copilot 工具；
- 将 Playwright MCP 服务器添加到工作区；
- 让 Copilot 在浏览器中手动测试筛选功能；
- 发布分支并为筛选工作创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

智能体模式可以访问编辑器以外的工具，因此功能更强大。Copilot 使用 Model Context Protocol (MCP) 与外部工具和服务通信。

![智能体模式的内部工作原理图，展示上下文、LLM 和工具之间的交互，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 是 AI 智能体与外部工具和服务通信的方式。智能体可以通过资源获取最新信息，并通过工具代表你执行操作。

MCP 服务器充当智能体与外部工具和服务之间的桥梁，负责管理与现有 API 或本地工具（例如 NPM 包）的通信。每个 MCP 服务器都提供一组工具和资源。

常见服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供管理 GitHub 存储库、issue 和拉取请求的 API。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，例如浏览网页、填写表单和选择按钮。

还有许多提供不同工具和资源的 MCP 服务器。GitHub 提供 [MCP 注册表](https://github.com/mcp)，方便发现和参与 MCP 生态系统。

> [!CAUTION]
> 对待 MCP 服务器应像对待项目中的其他依赖项一样。使用前仔细检查源代码、验证发布者并评估安全影响。只使用可信的 MCP 服务器，并谨慎授予敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 配置了此 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已配置的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板预先配置了 `github` 条目，因此 Copilot 能在前一练习读取 issue 待办列表。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装。VS Code 通过 HTTP 连接，并在 Copilot 首次使用其工具时要求你登录 GitHub。

## 添加 Playwright MCP 服务器

[Playwright MCP 服务器][playwright-mcp-server]可为 Copilot 提供可控浏览器，用于测试功能。

1. 在 `.vscode/mcp.json` 中，添加与 `github` 同级的 `playwright` 条目：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在本机运行 `npx @playwright/mcp@latest` 启动它。`--headless` 参数要求 Playwright 在后台运行浏览器。Codespace 没有可显示浏览器的桌面，因此必须使用该参数。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 运行端到端测试，通常已经安装 Playwright 所需的浏览器。如果 Copilot 报告缺少浏览器，让它运行 `npx playwright install chromium` 后重试。

## 启动并信任服务器

VS Code 会在需要时启动 MCP 服务器。Copilot 首次调用服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. `.vscode/mcp.json` 中的 `playwright` 条目上方会显示 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，先查看配置，再选择信任以启动服务器。

手动启动是可选的；若跳过，VS Code 会在下一步 Copilot 首次需要其工具时启动服务器并询问信任。

## 测试筛选功能

连接服务器并启动应用后，让 Copilot 操作浏览器测试刚构建的功能：

```text
使用 Playwright MCP 服务器打开 http://localhost:4321 上正在运行的应用，并验证新的游戏筛选功能：

1. 打开游戏页并记录列表中的游戏数量。
2. 应用类别筛选，确认列表只显示该类别的游戏。
3. 清除筛选，再应用发行商筛选，确认列表只显示该发行商的游戏。
4. 同时应用类别和发行商筛选，确认结果同时符合两个条件。

报告每一步的观察结果，并指出行为不符合预期的地方。
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐项测试并汇报结果。对照 issue 中的验收标准审查报告；如果发现问题，可继续提问或让 Copilot 修改代码，然后再创建 PR。

在执行测试时，应用必须运行于 `http://localhost:4321`。如果开发服务器已停止，请重新启动。Copilot 首次使用 Playwright MCP 时可能需要下载浏览器；若提示缺少浏览器，让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，发布 `filtering-vscode` 分支，供团队审查：

1. 打开 Codespace 中的 **Source Control** 面板，查看 Copilot 的变更。
2. 选择 **+** 图标暂存变更。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 变更的 Source Control 面板屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）创建 PR。也可以让 Copilot 使用已连接的 GitHub 工具查找相关 issue 并创建关联 PR：

1. 打开 Copilot Chat 面板并选择 **New Chat** 开始新会话。
2. 在智能体下拉列表中确认选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 发送以下提示：

   ```plaintext
   查找存储库中与按类别和发行商筛选相关的 issue。为当前分支创建新的拉取请求，并关联正确的 issue。
   ```

4. 按需选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时，可能会提示你登录 GitHub。
5. 确认 Copilot 找到了正确的 issue 并创建 PR。
6. 选择 Copilot 返回的链接审查 PR，**暂时不要合并**。

## 总结和下一步

本练习中，你了解了 MCP 和 MCP 服务器如何扩展 Copilot 工具，将 Playwright MCP 服务器添加到工作区，使用它在浏览器中测试筛选功能，并发布分支、创建筛选功能 PR。

MCP 服务器可以帮助测试功能，但它只是扩展 Copilot 的一种方式。下一步创建[自定义智能体][next-lesson]，简化无障碍审查等专注任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
