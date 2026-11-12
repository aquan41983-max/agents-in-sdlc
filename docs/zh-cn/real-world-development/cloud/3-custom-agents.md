---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中通过 Markdown 文件定义智能体，为其提供专门的指导、最佳实践、编码模式和领域知识。例如，团队可以创建用于执行 [WCAG][wcag] 规范的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含至少有 `name` 和 `description` 字段的 YAML frontmatter，以及用于定义行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行者，技能是工具**。

自定义智能体拥有自己的上下文窗口，并负责协调技能（甚至其他智能体）完成工作。本实验中的无障碍自定义智能体会根据无障碍指南审查和更新网站，也可以调用 PR 工作流或测试管理等技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。和其他 AI 功能一样，应在自己的环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本练习将介绍如何：

- 定义自定义智能体；
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，不受视力能力或偏好影响。近期用户反馈指出，目前的暗色主题对比度不足，文字难以辨认。设计团队因此要求添加可切换的高对比度模式。

无障碍至关重要，因此将使用自定义智能体快速生成此功能。

## 查看无障碍自定义智能体

已为无障碍任务创建自定义智能体。先查看它的内容，了解它将如何引导 Copilot。

返回 Codespace，并检查无障碍自定义智能体：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 查看后续部分，了解生成无障碍网站代码时的核心职责、最佳实践、HTML/CSS/JavaScript 示例，以及常见问题。

## 创建并分配 issue

“Mission control”是管理环境中所有智能体的中心位置。你可以在此分配任务、监控任务，并重新引导正在进行的会话。现在创建一个 issue，让 Copilot 添加高对比度模式：

1. 打开存储库。
2. 选择 **Issues** 选项卡，再选择 **New issue** 和 **Blank issue**。
3. 将 **Title** 设为 `Add high contrast mode to website`。
4. 将 **Description** 设为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

5. 选择 **Create** 创建 issue。
6. 在右侧选择 **Assign to Copilot**，再从自定义智能体列表中选择 **Accessibility agent**。

    ![分配 cloud agent 时选择并突出显示自定义智能体和无障碍智能体的屏幕截图](../../../_images/ex5-select-custom-agent.png)

7. 选择 **Assign**。Copilot 会在后台开始处理任务。

## 总结和下一步

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，以及如何通过专用 AI 助手将团队专业知识和标准转化为可复用的指导。

本课介绍了以下概念：

- 定义自定义智能体；
- 将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来前往 mission control，[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [在组织中使用自定义智能体的准备工作][org-custom-agents]
- [在企业中使用自定义智能体的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
