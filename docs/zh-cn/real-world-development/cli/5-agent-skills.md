---
title: "课程 5 - 自定义并使用 quality-checks 技能"
description: "查看现有的 quality-checks 技能、自定义报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码只是开发工作的一部分。我们已手动验证代码并使用说明文件确保遵循规范，接下来还要处理测试、lint 和持续集成（CI）的其他部分。

**智能体技能**非常适合这类任务！技能可以告诉 Copilot 如何正确执行操作。

本课将介绍如何：

- 查看现有 `quality-checks` 技能；
- 自定义技能结果的报告格式；
- 重新载入并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更容易阅读。

## 说明、脚本和资源

智能体技能将可复用的任务说明、可执行脚本和支持资源打包在一起，由智能体按需载入。每项技能的核心是一个同名文件夹，其中包含 `SKILL.md` 文件。该 Markdown 文件的 frontmatter 使用名称和描述定义技能，正文则说明技能用途以及何时调用。文件夹也可以包含脚本和其他资源。

> [!NOTE]
> 技能不一定需要额外文件和文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目已有命令。

技能可以放在项目 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 查看技能

查看 Tailspin Toys 团队用于运行单元测试、lint 和类型检查的 `quality-checks` 技能：

1. 返回 Codespace，在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 查看顶部的 `name` 和 `description`。描述帮助 Copilot 判断何时调用技能。
3. 阅读说明，了解它如何引导 Copilot 执行测试和 lint。
4. 注意技能当前还没有 **Results output formatting** 部分。

## 修改前先运行技能

技能可直接通过 Copilot CLI 调用，也可以使用自然语言请求。现在让 Copilot 运行三项检查：

1. 返回处于 Interactive 模式的筛选会话。
2. 输入以下提示：

   ```plaintext
   运行 quality-checks 技能，执行单元测试、lint 和类型检查。
   ```

3. 查看最终报告。

## 自定义报告

我们希望报告能说明运行了什么、是否成功，以及工具实际报告了什么。为技能添加报告要求：

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   完成后，报告运行过的每条命令及其结果（通过、失败或受阻）。仅当工具提供测试数、持续时间、错误、警告或其他指标时才包含这些信息。指出失败或受阻时的下一步操作，绝不要将跳过或未完成的检查描述为通过。
   ```

3. 文件会自动保存。

## 运行更新后的技能

Copilot CLI 无需重启对话即可重新载入编辑过的技能：

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 再次输入完全相同的提示：

   ```plaintext
   运行 quality-checks 技能，执行单元测试、lint 和类型检查。
   ```

3. 查看最终报告并与第一次的结果比较。

## 总结和下一步

你已自定义并使用现有智能体技能。本课中，你查看了 `quality-checks` 技能如何执行单元测试、lint 和类型检查，自定义结果格式，并重新载入和运行技能。

此变更将与筛选功能一起纳入功能 PR。接下来使用 [Playwright MCP 服务器][next-lesson]让 Copilot 直接操作网站。

## 更多技能示例

以下社区示例仅供参考，不是额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表及其导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
