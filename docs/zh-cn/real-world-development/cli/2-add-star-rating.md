---
title: "课程 2 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发浏览器中检查变更，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试对话后，现在来为项目做第一个小改动。游戏数据中已有星级评分，但主页的游戏卡片尚未显示。让智能体显示已有评分，检查变更，并将其作为第一个拉取请求合并。

本课将介绍如何：

- 在功能分支中开始有明确目标的 Copilot 对话；
- 让智能体对项目进行小幅修改；
- 使用 `/diff` 审查变更；
- 运行应用，并在转发的浏览器中确认变更；
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，而且游戏详情页已显示评分。不过，主页的游戏卡片只显示标题、类别、发行商和描述。作为热身练习，让智能体在每张卡片上显示已有评分。这项小改动范围明确，非常适合首次使用会话。

## 对话的组成

**对话**是你使用 Copilot CLI 处理任务的地方。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可供日后继续使用；文件和分支仍是磁盘上的常规 Git 状态。

在对话中可以看到提示和智能体回复、智能体探索和编辑文件时的工具活动，以及可通过 `/diff` 检查的变更。

## 开始对话并请求改动

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，执行：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，开始新会话。
5. 输入以下提示：

   ```plaintext
   在游戏列表页的每张游戏卡片上显示 starRating（满分为 5 分）。如果评分为 null，则显示“暂时没有评分”。保持卡片布局不变，添加测试并运行相关检查。
   ```

Copilot 会检查项目，找到显示游戏详情的文件并编写所需代码。

## 审查差异

合并前必须审查所有 AI 生成的变更，即使改动很小也不例外：

1. 输入 `/diff` 并检查每个变更文件。
2. 确认游戏卡片在有评分时显示数值，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种情况。
4. 检查 Copilot 运行的检查结果，并要求它修复失败项。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> Copilot 和其他生成式 AI 工具一样具有概率性，因此生成的代码可能不同。审查实际行为，不要期待某一种固定实现。

## 检查变更

不能只阅读代码并假设它能正常工作。让 Copilot 启动网站，再通过 Codespaces 转发的浏览器查看更新后的界面。

1. 让 Copilot 启动应用：

   ```plaintext
   启动应用，让我在浏览器中检查星级评分变更。告诉我 URL，并让服务器保持运行。
   ```

2. 当 Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示满分为五分的评分。
4. 返回 Copilot 并让它停止所启动的服务器：

   ```plaintext
   停止你启动的开发服务器。
   ```

## 创建并合并第一个拉取请求

现在功能已完成，可以创建拉取请求（PR）并将代码合并到项目中。

1. 让默认智能体提交变更：

   ```plaintext
   使用合适的提交消息提交已审查的星级评分变更。
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL。
4. 检查变更文件和检查结果。
5. 准备好后，选择 **Merge pull request** 并确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main` 分支：

   ```bash
   git checkout main
   git pull
   ```

## 总结和下一步

恭喜，你已使用 GitHub Copilot CLI 发布了第一个变更！你在功能分支中开始专注的 Copilot 对话，让智能体修改游戏卡片，使用 `/diff` 审查变更，在转发浏览器中确认评分，并创建和合并了第一个 PR。

接下来，从筛选功能 issue 开始，在 [Plan 和 Autopilot 模式中构建更大的功能][next-lesson]。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
