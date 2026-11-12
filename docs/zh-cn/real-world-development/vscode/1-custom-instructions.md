---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能访问这些信息。[说明文件][instruction-files]是提供代码生成指导的主要方式。

本练习将介绍如何：

- 通过存储库自定义说明和按路径限定的说明文件，为 Copilot 提供项目上下文、编码指南和文档标准；
- 在现有说明下生成第一个筛选数据切片（发行商辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 再次运行相同提示，观察生成的代码如何采用新标准；
- 将说明更新和筛选代码切片提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不符合所有规范。Copilot 具有非确定性。本练习要观察的是更新说明后行为的*整体变化趋势*，而不是要求输出逐字一致。

## 说明文件

### 场景

Tailspin Toys 有一套开发规范，包括：

- 数据层必须有单元测试；
- UI 应采用暗色模式并具有现代感；
- 代码应包含 TSDoc 文档注释；
- 每个文件开头应有说明用途的注释块。

通过说明文件，可确保 Copilot 获得正确的背景信息，并遵循这些实践。

### 自定义说明

自定义说明为 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。你可以为整个存储库或特定文件类型指定编码规范、库及注释风格：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个项目级说明文件。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> IDE 中的说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。Copilot Chat、Copilot CLI 和 Copilot cloud agent 会使用存储库级说明文件，以及含 `applyTo` frontmatter 的 `*.instructions.md` 文件。Copilot 也支持 AGENTS.md、CLAUDE.md 等其他标准，详情见[说明文件支持参考][custom-instructions-support]。

### 管理说明文件的最佳实践

- 让 `copilot-instructions.md` 聚焦于项目级指导，如项目描述、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持说明清晰，并提供正反示例。

说明文件没有唯一正确的写法，可以通过实践找到适合项目的方式。

> [!TIP]
> 使用 GitHub Copilot 的项目都应有一组完善的说明文件。本项目提供了 UI 更新和 [Astro][astro-instructions]等任务的示例。
>
> Copilot 也可以帮你生成说明文件。不同界面的操作不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。如需模板，可查看包含说明文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot]。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目的自定义说明文件

项目附带一个核心 `copilot-instructions.md` 和一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中查看：

1. 打开 `.github/copilot-instructions.md`，查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。
2. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等说明。
3. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段；例如 `**/*.test.ts` 会匹配 TypeScript 测试文件。
4. 查看单元测试专用说明。
5. 打开 `.github/instructions/drizzle.instructions.md`，查看指向其他说明和项目文件的链接。相对路径以说明文件所在目录为基准。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分描述项目编码标准，但尚未要求代码内文档。下一步将添加 TSDoc 注释和文件头注释规范。

## 为变更创建分支

1. 返回上一练习创建的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat** 开始干净的对话。模式和模型因练习而异；每一步会说明具体设置。

先使用当前说明文件发送提示。更新说明后，再次发送相同提示以观察差异：

1. 关闭之前练习留下的编辑器选项卡，避免 Copilot 获得无关上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**。

   ![Chat 视图中的智能体选择器屏幕截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发行商的名称和 id。应用文件变更。
   ```

Copilot 会探索项目并修改辅助文件及其测试。留意生成的函数应采用类型定义、接收 `db` 客户端并返回发行商数组，这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md`。同时留意代码尚未包含 TSDoc 注释和文件级注释。

> [!CAUTION]
> Copilot 具有概率性，有时即使未明确要求也会添加文档注释。这没有问题；本练习的重点是更新说明后行为更一致。

## 添加新的存储库标准

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（通常接近第 27 行）。这里记录了项目编码标准，但尚未包含代码文档要求。
3. 在现有标准下方添加：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存文件。

> [!TIP]
> 存储库级 `.github/copilot-instructions.md` 适合放置全局标准；`*.instructions.md` 可用于语言、文件类型或任务专属指导。

## 再次运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除上下文。
2. 再次打开 `src/lib/publishers.ts`。
3. 发送之前的相同提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发行商的名称和 id。应用文件变更。
   ```

4. 查看生成的文件是否以说明其用途的注释块开头。
5. 确认函数包含 TSDoc 注释，说明用途、`db` 参数和返回值。

你已引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一练习会继续使用这段代码。

## 提交说明并推送分支

1. 在终端中运行以下命令，确认说明更新和辅助函数变更已就绪：

   ```bash
   git status
   ```

2. 暂存并提交说明更新及辅助函数文件。如果 `git status` 显示还有测试等相关变更，也将这些文件加入提交：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

3. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并关联待办列表中的说明标准 issue。可以让 Copilot 使用已连接的 GitHub 工具查找 issue 并创建 PR：

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat，并确认智能体选择器中选中 **Agent**。
3. 提示 Copilot 查找相关 issue，并从当前分支创建 PR，说明 PR 关闭该 issue，同时包含筛选工作所需的发行商辅助函数：

   ```plaintext
   查找与更新说明文件相关的 issue。从当前分支创建 PR，说明该 PR 会关闭此 issue，并说明我们还为后续筛选工作添加了发行商辅助函数基础。
   ```

4. Copilot 会查找 issue 并创建 PR。收到 GitHub 操作授权提示时，先审查命令，再选择 **Allow**。
5. 创建 PR 后，让 Copilot 将其合并到 `main`，并在本地切回 `main`、拉取最新代码：

   ```plaintext
   将 PR 合并到 main。然后在本地切换回 main 并拉取最新代码，确保代码已更新。
   ```

6. 收到 GitHub 操作和 shell 命令授权提示时，先审查命令，再按需选择 **Allow**。

现在已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和下一步

你了解了 Copilot 如何读取说明文件，并通过 VS Code 中的 Copilot Chat 使用已有说明生成代码、添加存储库级标准，再次运行相同提示观察变更，最后将说明更新和发行商辅助函数合并到 `main`。

接下来，在[智能体模式][next-lesson]中运用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件和其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
