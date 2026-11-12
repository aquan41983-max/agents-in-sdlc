---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能访问这些信息。[说明文件][instruction-files]可提供相关指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

本练习将介绍如何：

- 通过存储库自定义说明和按路径限定的说明文件，为 Copilot 提供项目上下文、编码指南和文档标准；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 路线不同，这里不会运行添加说明前后的对照提示。Copilot cloud agent 会异步处理 GitHub issue，因此难以实时并排展示效果。稍后审查它创建的拉取请求时，可以看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 有一套开发规范，包括：

- 数据层必须有单元测试。
- UI 应采用暗色模式并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头应有注释块，说明文件用途。

通过说明文件，可以确保 Copilot 获得正确的背景信息，并遵循这些实践。

### 自定义说明

自定义说明为 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。你可以指定编码规范、库以及注释风格，也可以为整个存储库或特定文件类型创建说明。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。内容应是与大多数对话或 CLI 请求相关的项目级信息，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时会使用存储库级说明文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的说明文件，例如 AGENTS.md 和 CLAUDE.md，详情见[说明文件支持参考][custom-instructions-support]。

### 管理说明文件的最佳实践

本工作坊不会完整讲解如何编写说明文件，但示例项目展示了一种常见做法：

- 让 `copilot-instructions.md` 聚焦于项目级指导，例如项目简介、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型或任务提供具体指导。
- 使用自然语言，让指导清晰，并提供正反示例。

说明文件没有唯一的正确写法。通过实践，找到适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一组完善的说明文件。浏览本项目中的文件时，你会看到许多不同任务对应的说明，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮你生成说明文件。不同产品界面的操作不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相应课程会在需要时介绍。
>
> 如需模板或入门示例，可以查看 [awesome-copilot][awesome-copilot]，其中收录了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目的自定义说明文件

查看此存储库随附的说明文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 中还包含 **GitHub Actions Workflows** 指导，这些内容适用于 Copilot 在此存储库中的所有交互。
3. 打开 `.github/instructions` 文件夹，查看其中的说明。你会发现 Astro、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它指定相对于存储库根目录的 glob；例如 `**/*.test.ts` 会匹配 TypeScript 测试文件。
5. 查看有关为本项目编写单元测试的具体要求。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。留意指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明拆分成可复用的小文件，并提供示例（其中路径相对于说明文件所在目录）。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分描述项目编码标准，但目前尚未要求在代码中添加文档注释。接下来会添加 TSDoc 注释和文件头注释规范。

## 添加新的存储库标准

接下来要添加本练习唯一的编辑内容：规定代码应包含 TSDoc 文档注释和文件级注释。稍后分配 issue 时，cloud agent 会读取这项规范。

先创建工作分支。cloud agent 会读取 issue 目标分支中的说明，不过这里仍会按常规流程提交变更：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于提供项目级信息。现在为代码建议补充存储库编码标准：

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（通常接近第 27 行）。它记录了项目编码标准，但目前缺少代码文档要求。
3. 在现有标准下方添加以下 Markdown 内容：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 说明文件既可放在存储库级别的 `.github/copilot-instructions.md` 中，提供全局指导；也可使用 `*.instructions.md` 文件为语言、文件类型或任务提供具体指导。存储库级文件适合放置刚添加的项目级标准。

## 提交、推送并合并说明更新

cloud agent 会读取 issue 目标分支中的说明。在下一练习中分配 issue 时，Copilot 会从 `main` 创建分支，因此说明变更必须先合并到 `main`。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。之后 cloud agent 处理 issue 时就会读取这些说明。

> [!TIP]
> 如果想在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处保留分支步骤，是为了模拟真实项目的工作方式。

## 总结和下一步

你了解了 Copilot 如何读取本项目的说明文件，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。接下来的课程中，可以在 cloud agent 创建的拉取请求中看到该标准的效果。

接下来，将[第一个 issue 分配给 Copilot cloud agent][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件和其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
