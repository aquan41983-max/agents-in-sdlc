---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发者需要找到相关文件、进行修改、运行测试、调试、发现遗漏并再次修改，过程可能很繁琐。

Copilot 智能体模式可以更自主地在 IDE 中工作。它会探索项目结构、进行必要更新、运行测试，并自动修复发现的问题。接下来使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和 cloud agent 名称相似，但面向不同场景。智能体模式在 IDE 中执行任务，可快速反馈并随时交互；cloud agent 则像团队成员一样异步工作，通过 issue 和拉取请求与你协作。

本练习将介绍如何：

- 让 Copilot 智能体模式探索项目、查找相关文件并协同修改；
- 跨 UI 和数据层实现新功能；
- 在合并前审查 Copilot 生成的变更和测试。

## 场景

随着游戏列表不断增加，你希望用户可以按类别和发行商筛选。上一练习已添加发行商辅助函数；现在将使用 Copilot 智能体模式完成数据层、UI 和测试的其余工作。

## 运行 Tailspin Toys 网站

修改前先了解网站当前功能。Tailspin Toys 是以开发者为主题的桌游众筹平台，可列出游戏并显示详细信息。它是一个 Astro 应用，在构建时将页面生成为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有独立后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能使用标准 DOM API 编写小型、范围明确的 Astro `<script>`。

### 启动网站

`npm run dev` 会启动唯一的 Astro 开发服务器。按照以下步骤运行：

1. 返回上一练习使用的 Codespace。此时应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
3. 更新代码并创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似如下信息：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 `http://localhost:4321`。

> [!NOTE]
> 在 Codespace 中选择终端中的 localhost 链接时，会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向当前 Codespace 的专用隧道，网站服务器正在其中运行。

如果弹出询问是否打开 `http://localhost:4321` 的对话框，选择 **x** 将其关闭。

### 浏览网站

网站运行后，可以浏览其功能：

- **主页**：显示桌游列表，包括标题、图片和描述；
- **游戏详情页**：选择游戏后查看标题、描述、发行商和类别等详情。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat** 开始干净的对话。模式和模型因练习而异；每一步会说明具体设置。

网站的初始实现已能正常工作，但还可以添加功能。让 GitHub Copilot 查看从项目模板自动创建的待办 issue：

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。该智能体会自主规划和实现跨文件变更、运行终端命令并调用工具。

   ![Chat 视图中的智能体选择器屏幕截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某一种模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将其列入说明，以便获得较一致的体验。由于大型语言模型具有概率性，你得到的建议可能与本工作坊展示的不同，这是正常现象。

3. 发送以下提示：

   ```plaintext
   请列出我的 GitHub 存储库中的待办事项，并根据对用户的帮助程度协助我确定优先级。
   ```

4. 选择 **Continue** 以运行列出所有 issue 的命令，然后查看结果。

## 查看说明文件

生成代码前，先查看用于提供上下文的说明文件。此练习使用[用户界面 (UI) 说明][ui-instructions]，了解如何为网站添加功能：

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看总体指导，包括架构、组件设计、可测试性、无障碍，以及指向 Astro、Tailwind CSS 和 Drizzle 等专用说明的链接。

> [!TIP]
> 说明文件可引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以复用指导、拆分较大的说明，并引用示例。

## 实现筛选功能

筛选功能至少需要完成以下修改：

- 在数据层（`src/lib/`）添加或完善筛选逻辑；
- 添加或更新筛选测试；
- 更新游戏列表页并添加筛选 UI。

Copilot 智能体模式可以协助完成这些任务：

1. 继续当前对话，或选择 **New Chat**。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图中选中了 **Agent**，并继续使用 **Claude Sonnet 4.5**。
4. 使用待办列表中的相关 issue 提示 Copilot：

   ```plaintext
   请根据待办列表中相关 GitHub issue 的要求，为网站添加按发行商和类别筛选的功能。上一练习已创建发行商辅助函数，请保留并酌情完善，同时完成数据层、UI 和测试的其余工作。完成前确保所有测试通过。服务器已在运行，无需再次启动。
   ```

5. 观察 Copilot 探索项目并查找数据层、UI 和测试文件，然后进行修改并运行测试。

   ![Copilot 探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试，可能需要几分钟。过程中编辑器可能显示 **Keep** 和 **Undo**；完成前无需选择它们。

6. Copilot 提示运行测试时，选择 **Continue**。
7. 处理过程中可能暂停或出现测试失败，这是正常情况。Copilot 会在生成代码和运行测试之间迭代，直至不再发现错误。
8. 查看生成的代码是否有潜在问题。

> [!CAUTION]
> 始终审查 Copilot 或其他 AI 工具生成的代码。

9. 返回正在运行的网站，探索新功能。
10. 确认功能正常且代码审查完成后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和下一步

你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了功能。本课中，你了解了智能体模式如何探索项目、查找相关文件并协同修改 UI 和数据层，也在合并前审查了生成的变更和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外练习 — 实现分页

随着游戏列表增长，网站将需要分页。运用本练习所学知识，让 Copilot 添加分页功能。编写提示时，考虑分页方式、是否允许用户选择每页大小、应遵循的说明文件，以及需要提供哪些上下文。与 Copilot 迭代、要求修改并补充上下文是正常工作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：用途及选择时机][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
