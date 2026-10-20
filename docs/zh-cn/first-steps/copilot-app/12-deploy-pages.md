---
title: "课程 12 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查拉取请求，并确认 Space Quiz 已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 发布 Space Quiz，让所有人都能访问。Copilot 可以准备工作流并引导部署，但你仍需负责审查更改并确认网站已上线。

本课程将完成以下任务：

- 请求 Copilot 配置 GitHub Pages 部署。
- 合并前审查工作流和拉取请求。
- 跟进部署过程并验证已发布的网站。

## 场景

Space Quiz 已位于 GitHub 仓库中，但仍需要一种简单的体验方式。使用 GitHub Pages 发布这个单文件网站后，用户就能通过公开 URL 访问，而且无需添加服务器或运行时依赖项。

## 请求 Copilot 部署网站

在 `space-quiz` 的 Copilot app 会话中发送以下提示：

```plaintext
为此仓库启用 GitHub Pages。创建一个 GitHub Actions 工作流，在需要时构建网站并将其部署到 GitHub Pages。如果尚无 README，请添加一个文件，介绍此应用以及如何在本地运行。使用这些更改创建拉取请求，并在必需的检查通过后合并。持续监控部署，直到成功，然后报告已上线网站的最终 URL。如果你没有启用 Pages 或合并的权限，请准确说明我需要批准或执行的操作；网站上线之前不要报告成功。
```

![三阶段示意图：space-quiz 拉取请求已合并；GitHub Actions 的 Deploy website 运行成功，包含 Build website、Upload artifact 和 Deploy to Pages 步骤；Space Quiz 已在 yourname.github.io/space-quiz/ 上线。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示要求智能体创建并合并拉取请求，以及更改仓库的 Pages 设置。继续之前，请审查差异、部署权限和 GitHub 的所有审批请求。

## 审查工作流并验证部署

1. 确认只有在仓库尚无 README 时才添加了 README。
2. 审查拉取请求中的更改，包括工作流的构建和部署步骤。
3. 只有在必需的拉取请求检查通过后才合并。
4. 跟进 GitHub Actions 部署运行，直到其成功完成。
5. 打开报告中的 URL，确认 Space Quiz 可以加载。

如果智能体无法启用 Pages 或合并拉取请求，请按照它提供的具体说明操作。部署成功且网站正常加载之前，不要将任务视为已完成。

## 总结与后续步骤

现在 Space Quiz 已作为网站发布，并且你亲自验证了部署，而不是只依赖智能体的报告。继续学习[课程 13：回顾与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
