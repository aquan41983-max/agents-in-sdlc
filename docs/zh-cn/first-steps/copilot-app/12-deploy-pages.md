---
title: "课程 12 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查拉取请求，并验证 Space Quiz 已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 部署 Space Quiz，为它提供公开访问地址。Copilot 可以准备工作流并引导部署，但你仍需负责审查变更并确认网站已上线。

本课将介绍如何：

- 让 Copilot 配置 GitHub Pages 部署；
- 在合并前审查工作流和拉取请求；
- 跟进部署并验证已发布的网站。

## 场景

Space Quiz 已在 GitHub 存储库中，但用户还没有简单的试用方式。使用 GitHub Pages 发布单文件网站，无需添加服务器或运行时依赖，即可通过公开 URL 访问。

## 让 Copilot 部署网站

在 Copilot app 中打开 `space-quiz` 会话，并发送以下提示：

```plaintext
为此存储库启用 GitHub Pages。创建一个 GitHub Actions 工作流（如有需要则先构建网站）并将网站部署到 GitHub Pages。如果存储库中尚无 README，请添加一个，介绍应用以及如何在本地运行。使用这些变更创建拉取请求，并在必需检查通过后合并。持续监控部署，直到成功，然后报告线上网站的最终 URL。如果你没有启用 Pages 或合并的权限，请准确说明我需要批准或执行的操作；在网站上线前不要报告成功。
```

![三阶段示意图：已合并的 space-quiz 拉取请求；包含 Build website、Upload artifact 和 Deploy to Pages 步骤的成功 GitHub Actions Deploy website 运行；以及 yourname.github.io/space-quiz/ 上线的 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示要求智能体创建并合并拉取请求，以及更改存储库的 Pages 设置。继续之前，请审查差异、部署权限和 GitHub 的所有审批提示。

## 审查工作流并验证部署

1. 确认只有在存储库尚无 README 时才添加 README。
2. 审查拉取请求中的变更，包括工作流的构建和部署步骤。
3. 只有必需的拉取请求检查通过后才合并。
4. 跟进 GitHub Actions 部署运行，直到成功。
5. 打开报告中的 URL，确认 Space Quiz 已加载。

如果智能体无法启用 Pages 或合并拉取请求，请按照它提供的具体说明操作。只有部署成功且线上网站可正常加载后，任务才算完成。

## 总结和下一步

Space Quiz 现已作为网站上线。你也亲自验证了部署，而不是仅依赖智能体的报告。继续学习[课程 13：回顾和后续步骤][next-lesson]。

[next-lesson]: ../13-review/
