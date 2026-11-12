---
title: "课程 7 - 创建并使用 QA 智能体"
description: "创建结合 issue 需求、quality-checks 技能和 Playwright MCP 的 QA 自定义智能体。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并通过 Playwright MCP 在浏览器中观察筛选功能。现在将这些能力整合到一个自定义智能体中，建立清晰的 QA 流程。

本课将介绍如何：

- 了解自定义智能体如何与说明、技能和 MCP 工具协作；
- 创建并检查可复用的质量保证配置；
- 选择 QA 智能体，并根据筛选 issue 审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，一致地检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本，其中说明智能体的用途、指令和可用工具。本工作坊将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

此前使用的自定义项各有用途：存储库说明描述团队规范；`quality-checks` 技能打包可重复的检查；Playwright MCP 提供浏览器工具；QA 配置则指导 Copilot 使用这些能力检查需求并报告结果。它们无需取代彼此，也无需开启另一段对话。

## 创建 QA 配置

创建功能 PR 前，让 Copilot 创建可复用的 QA 配置。该配置将定义 QA 执行的检查及其工作边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 让默认智能体创建配置：

   ```plaintext
   在 .github/agents/qa.agent.md 中创建名为 QA 的自定义智能体。它应根据 issue 和已商定的需求检查功能，遵循存储库说明，运行 quality-checks 技能，使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   对每项需求标记通过、失败或受阻，并提供支持证据。修改实现代码前必须先询问；不得提交变更或创建拉取请求。

   现在只创建配置，供我审查。
   ```

## 检查配置

使用新智能体前，先确认 Copilot 捕获了预期的 QA 流程和边界：

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 查看 frontmatter。`description` 为必填项；`name` 为可选项，但添加后显示名称更清晰。
3. 确认说明要求 QA 从需求开始，遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认报告包含支持证据；修改实现代码前先征求同意；并且不会提交变更或创建 PR。
5. 如果缺少任何职责或边界，请让默认智能体先修订。

## 根据 issue 运行 QA

Copilot CLI 会在对话开始时载入项目智能体。创建配置后，恢复同一筛选对话并选择 QA，以便它使用已有的 issue 和规划决定：

1. 输入以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会立即出现在可用智能体列表中，但仍可使用上述命令启用。

2. 输入以下提示，让 QA 智能体检查功能：

   ```plaintext
   根据 issue 和计划中的决定审查筛选功能。它是否已准备好创建 PR？
   ```

3. 等待 QA 智能体完成工作，然后阅读报告。

## 总结和下一步

你添加了可复用的专用角色，并审查了它的工作。本课中，你了解了自定义智能体如何与说明、技能及 MCP 工具协作，创建并检查 QA 配置，并根据筛选 issue 审查报告。

现在实现、技能更新、QA 配置、测试和验证报告均已准备好审查。下一课将[汇总这些内容到功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
