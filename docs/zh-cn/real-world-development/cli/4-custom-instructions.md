---
title: "课程 4 - 使用自定义说明引导 Copilot"
description: "查看存储库说明、添加文档标准，并将该标准应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，应确保 Copilot 能获取相应指导。[说明文件][instruction-files]不仅描述想要什么代码，也描述代码应如何组织。完成筛选功能后，现在来查看 Copilot 使用的说明、添加文档标准并将其应用于代码。

本课将介绍如何：

- 了解存储库说明和按路径限定的说明文件如何提供给智能体；
- 更新说明文件以确保遵循编码标准；
- 观察说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发规范，包括：

- 注释应解释意图和不明显的决策，而不是复述代码；
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值，并记录可注入的 `db` 参数；
- 可复用 Astro 组件应说明 `Props` 接口，相关代码变更时也应更新注释；
- 保留现有格式和 lint 规范。

通过说明文件，可以确保 Copilot 知道这些实践，并据此完成任务。

## 说明文件

自定义说明为 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。你可以为整个存储库或特定文件类型指定编码规范、库及注释风格：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件，适合存放与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。使用特定格式前，请查看[自定义说明支持参考][custom-instructions-support]。

## 查看本项目的自定义说明文件

入门项目已包含一组说明文件。先查看现有说明，再进行修改以观察效果：

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。它们适用于对此存储库进行的所有 Copilot 交互。
4. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段以及它指定的文件匹配范围。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看指向其他说明文件和项目现有文件的链接。

## 更新说明文件以符合团队指导

要修改核心 `copilot-instructions.md`，确保新生成的 TypeScript 代码包含 TSDoc 注释：

1. 找到名为 **Code formatting guidance** 的部分（通常接近第 35 行）。
2. 在末尾添加以下 Markdown 内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时载入存储库说明。保存更新后，退出并恢复筛选会话，这样 Copilot 就能在保留功能上下文的同时载入新指导。

1. 若筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选对话并确认当前检出的是 `game-filters-cli` 分支。
3. 要求 Copilot 应用更新后的指导：

   ```plaintext
   我们刚更新了说明和代码指导。请更新你生成的代码，使其符合这些指导。
   ```

4. 输入 `/diff` 并检查变更的 TypeScript 文件。留意新增的 TSDoc 注释，并确认其准确描述代码。

## 总结和下一步

你了解了 Copilot CLI 如何读取说明文件，并将新标准应用到筛选功能。本课中，你查看了存储库和路径范围说明、更新了编码标准，并观察该标准对代码的影响。

接下来使用可复用的 [quality-checks 技能][next-lesson]，确保以一致的方式运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件和其他资源][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
