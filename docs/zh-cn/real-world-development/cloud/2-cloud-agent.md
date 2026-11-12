---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

许多组织都面临技术债务，例如未解决的问题、需要更新的旧代码，或长期搁置的功能请求。GitHub Copilot cloud agent 可以自主更新代码和添加功能。完成任务后，它会创建草稿 PR，供开发者审查。这样可以把繁琐任务交给 Copilot，让开发者专注于更重要的工作。

本练习将介绍如何：

- 自定义代码生成环境；
- 确保操作安全；
- 编写范围明确的 issue；
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初承包网站的团队没有留下文档，因此首先需要为应用中所有导出的函数添加 TSDoc 文档注释。

设计团队还希望改进游戏发现功能：在每款游戏的详情页显示同类别的其他游戏，方便用户继续浏览。当前只需要用于验收测试用户体验的初步实现。此项工作暂时被阻塞，但优先级较高的工作还在排队。

这类工作容易被推迟，很适合分配给 Copilot cloud agent。它可以异步处理任务，让开发者继续完成其他工作，之后再回来审查结果。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发人员一样在后台执行任务。你可以通过[分配 GitHub issue][assign-issue]启动它。收到任务后，Copilot 会创建草稿拉取请求、准备环境并开始工作。你可以在执行期间或完成后查看会话；方案准备好供审查时，它会在 PR 中通知你。

## 明确说明任务范围的重要性

GitHub Copilot 没有魔法。即使看似简单的操作，也可能包含复杂细节。因此，为 cloud agent 设定任务范围时要遵循[最佳实践][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、学习、试验并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其作用。如果你已完成相关练习，就已经见过 `.github/copilot-instructions.md`；否则现在可以快速查看。分配工作前，先只读浏览此存储库中的说明文件，之后便能对照观察它们的影响。

在 GitHub 网页界面或 Codespace 中打开：

- `.github/copilot-instructions.md`：查看 **Code standards**，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将说明限定到 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` issue 分配给 cloud agent 时，观察生成的 PR 是否遵循这些说明。

## 为 Copilot cloud agent 设置开发环境

创建代码通常需要准备特定环境并运行设置脚本。将任务分配给 Copilot 时也是如此。cloud agent 使用 [GitHub Actions][github-actions] 执行工作；你可以在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]，预先安装工具和依赖项。

实验室已配置好工作流，让 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器，以及已迁移和填充数据的 SQLite 数据库。设置文件如下：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅设置步骤所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 contents: read 权限。如果未在设置步骤中克隆存储库，Copilot 会在设置步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置：Node.js（应用现为 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"
      - name: Install JavaScript dependencies
        run: npm ci
      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有数据可用。
      - name: Set up the database
        run: npm run db:setup
```

此工作流与其他 GitHub 工作流类似，但有两点值得注意：

- 它只有一个 `copilot-setup-steps` 作业，在 Copilot 开始 PR 前运行。
- `workflow_dispatch` 触发器可让你从存储库的 **Actions** 选项卡手动运行工作流，以便测试。

## 添加文档

大多数项目都存在文档过时或缺失的问题。这类技术债务会降低效率，也会增加维护代码库或帮助新成员上手的难度。Copilot 很适合处理文档任务，因此可以把此 issue 分配给 cloud agent，稍后再审查它生成的文档。

1. 在新浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，再选择 **Blank issue**。
4. 将 **Title** 设为 `Code lacks documentation`。
5. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

6. 选择 **Create** 创建 issue。
7. 在右侧选择 **Assign to Copilot**，再选择 **Assign**。

    ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

8. 选择 **Pull Requests** 选项卡，打开新建的 PR（标题可能类似 `[WIP]: Code lacks documentation`）。如果还没有 PR，请稍等并刷新页面。
9. 等待 Copilot 在 PR 中创建待办列表，再检查它准备完成的任务。
10. 向下查看 PR 时间线，确认 Copilot 已开始处理 issue。
11. 选择 **View session** 查看实时会话。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态。

你可以查看 Copilot 如何探索代码、了解当前状态、制定方案并编写代码。整个过程可能需要几分钟。cloud agent 的目标之一就是异步执行任务，让你能同时处理其他工作。接下来，再分配一项任务，然后编写应用功能。

## 在游戏详情页添加相关游戏

cloud agent 的一大优势是可以并行处理工作。即使设计团队要求的相关游戏功能不复杂，也可以把它交给 cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡，再选择 **New issue** 和 **Blank issue**。
3. 将 **Title** 设为 `Show related games on the game details page`。
4. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

    - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
    - Show a "Related games" section on the game details page that uses the helper
    - Handle the case where a game has no related games
    - There should be unit tests created for the new helper
    - Before creating the PR, ensure all tests pass
    ```

5. 选择 **Create** 创建 issue，然后在右侧选择 **Assign to Copilot** 并确认 **Assign**。

issue 的第一条评论中很快会出现 👀，表示 Copilot 已开始处理。它正在处理第二个请求。cloud agent 的工作方式类似软件工程师，不需要持续监控；完成后再审查即可。接下来了解自定义智能体。

## 总结和下一步

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。可以将 issue 分配给 Copilot，让它异步处理技术债务、开发功能或迁移框架。

本课介绍了以下概念：

- 自定义代码生成环境；
- 确保操作安全；
- 明确界定 issue 范围；
- 将 issue 分配给 Copilot。

cloud agent 在后台工作时，可以开始创建和使用自定义智能体。[Copilot cloud agent 也支持 MCP 服务器][cloud-agent-mcp]，并可使用前面练习中配置的自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
