---
title: "课程 3 - 智能体模式：Plan 和 Autopilot"
description: "先用 Plan 商定方案，再用 Autopilot 根据 issue 实现筛选功能，最后通过 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一个小功能，但更大的改动需要更可靠的流程。GitHub Copilot CLI 可以融入组织现有的开发流程，帮助团队以正确方式构建正确功能。本课将从 issue 开始，遵循典型的智能体开发流程：生成新功能、验证代码、检查行为，最后成功合并到项目中。

> [!NOTE]
> 后续课程会继续使用同一对话和分支。实际工作中，通常会为不同文件类型创建不同分支或 PR；此处为聚焦核心概念而简化流程。

本课将介绍如何：

- 从 GitHub issue 开始新的 Copilot 对话；
- 在 Plan 模式中定义需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发浏览器中手动验证功能。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办 issue 描述了功能，但组合类别等细节需要先达成一致。你将使用 Plan 模式确定这些决策，再授权 Copilot 在明确范围内实现功能。

## 背景

将 AI 编码智能体引入开发流程不会改变基本原则，反而使这些原则更加重要。常见开发流程如下：

1. 从描述需求的 issue 开始；
2. 制定实现计划；
3. 编写并审查代码；
4. 运行测试以验证代码；
5. 手动验证新功能；
6. 创建拉取请求（PR）；
7. 代码审查和持续集成成功后合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多与上述流程类似。

遵循标准流程可以确保 AI 生成的代码符合要求，并像手写代码一样经过审查。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出变更建议，并等待你的输入后再继续。
- **Plan**：智能体先创建计划，且不能编辑项目文件。
- **Autopilot**：智能体自主编写代码、运行测试并反复迭代，直至任务完成。

先使用 Plan 模式审查计划，再使用 Autopilot 实现计划。

## 从 issue 开始

开始筛选工作前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 若终端未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main` 并创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 连按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用方向键选中 **Allow users to filter games by category and publisher**。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中并返回 **Session** 选项卡。

注意提示现在以 `#7`（或类似编号）开头。使用 `#` 可将 GitHub issue 或拉取请求（PR）添加到上下文。

## 规划筛选功能

规划可以先确定功能实现方式，再交给 Copilot 执行。对于复杂任务，预留时间规划通常很有帮助。切换到 Plan 模式并要求 Copilot 制定计划：

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方显示 **Plan**。
2. 在刚添加的 issue 引用后输入：

   ```plaintext
   为实现此功能制定计划。
   ```

Copilot 会先探索项目，再确定合适的实现方式。如果它询问筛选行为的细节，请根据自己的偏好作答。计划完成后：

3. 按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
4. 浏览计划，并要求 Copilot 修改不符合你决定的部分。

## 批准 Autopilot

计划编写并审查完成后，使用 Autopilot 进行实现。Autopilot 会让 Copilot 反复处理问题，直至认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)** 或已安装版本中名称相近的选项。
2. 确认提示下方显示 **Autopilot**。
3. 观察 Copilot 如何按照计划编写代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，应先解决设置问题，再将检查视为已完成。

## 审查并验证实现

和其他代码一样，生成的代码也必须在合并前审查：

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式，确认提示下方不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff` 并检查筛选功能实现和测试。
3. 对照 issue 和规划时达成的决定检查结果。
4. 审查完成后按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 解决任何失败项。

## 试用新功能

代码看起来不错，但实际运行情况如何？像之前一样启动应用，通过 Codespaces 转发端口打开网站：

1. 让 Copilot 启动应用：

   ```plaintext
   启动应用，让我在浏览器中试用筛选功能。告诉我 URL，并让服务器保持运行。
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别、发行商筛选，以及计划中商定的组合方式。
4. 确认重置和无结果状态符合 issue 和规划中的决定。
5. 返回 Codespace，并让 Copilot 停止它启动的开发服务器。

## 总结和下一步

你通过不同对话模式构建并审查了一项功能。本课中，你从 GitHub issue 开始，在 Plan 模式中定义需求，使用 Autopilot 模式实现功能，并审查代码和在转发浏览器中手动验证功能。

下一课将使用[自定义说明][next-lesson]深入了解代码生成，确保生成的代码符合已有规范。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
