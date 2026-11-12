---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中创建 Markdown 文件，可以为 Copilot 提供专门的指令、最佳实践、编码模式和领域知识。团队可以将专业知识整理成可复用的智能体，例如执行 [WCAG][wcag] 规范的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体在项目 `.github/agents` 文件夹中通过 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。文件需包含至少有 `name` 和 `description` 字段的 YAML frontmatter，以及用于定义行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]都主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行者，技能是工具**。

自定义智能体拥有自己的上下文窗口，并负责协调技能（甚至其他智能体）完成工作。本实验中的无障碍自定义智能体会根据无障碍指南审查和更新网站，也可以调用 PR 工作流或测试管理等技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。应在自己的环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，不受视力能力或偏好影响。近期用户反馈指出，目前的暗色主题对比度不足，文字难以辨认。设计团队因此要求添加可切换的高对比度模式。

本练习将介绍如何：

- 审查现有无障碍自定义智能体；
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 查看无障碍自定义智能体

已为无障碍任务创建自定义智能体。先查看它的内容，了解它将如何引导 Copilot。

返回 Codespace，在终端中从 `main` 创建无障碍功能分支（练习 3 中的筛选 PR 将保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 查看后续部分，了解生成无障碍网站代码时的核心职责、最佳实践、HTML/CSS/JavaScript 示例，以及常见问题。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的自定义智能体。选择智能体后，该会话会采用其说明和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat** 开始干净的对话。模式和模型因练习而异；每一步会说明具体设置。

1. 在 Chat 视图的智能体下拉列表中确认选中 **Agent**。

   ![Chat 视图中的智能体选择器屏幕截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表；它显示当前智能体，默认是 **default**。
3. 在可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    为网站添加高对比度模式。应有一个可由用户切换的高对比度开关，并使用浏览器本地存储让设置在刷新页面后仍然保留。
    ```

5. Copilot Chat 会探索代码库、提出变更建议并编辑项目文件。每项编辑都会以内联差异的形式显示文件路径和内容。
6. **Files changed** 指示器会更新，显示智能体修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会修改 Astro 组件、CSS 和相关测试等现有文件。

下一课将审查并引导这项正在进行的工作。

## 总结和下一步

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，以及如何通过专用 AI 助手将团队专业知识和标准转化为可复用的指导。本课中，你了解了自定义智能体的定义方式，并在 Copilot Chat 智能体模式中使用它。

下一步[监控并引导智能体工作][next-lesson]，查看变更，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 中的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
