---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是专为开发工作流中特定任务或领域定制的人工智能（AI）助手。将智能体定义为存储库 `.github/agents` 文件夹中的 Markdown 文件，即可提供有针对性的指令、最佳实践、编码模式和领域知识，引导 Copilot 更有效地完成特定类型的工作。团队可以将专业知识整理成可复用的智能体，例如确保符合 Web 内容无障碍指南（WCAG）的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局存放在 `~/.copilot/agents`。每个文件都包含 YAML frontmatter（至少包含 `name` 和 `description`），后面是定义智能体行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]有一些相似之处：两者主要都由 Markdown 文件定义，并指导 AI 执行操作。最简单的区分方式是：**自定义智能体是执行者，技能是工具**。

自定义智能体拥有自己的上下文窗口，并可协调技能（甚至其他智能体）完成工作。本实验中的无障碍自定义智能体会根据无障碍指南审查和更新网站；执行过程中，它可以调用拉取请求（PR）工作流技能或运行并管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 相关的其他工作一样，需要针对不同环境和场景测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不论用户的视觉能力或偏好如何。近期反馈指出，有些用户觉得当前深色主题难以阅读，因为文字和背景颜色对比度不足。设计团队希望实现可以由用户切换的高对比度模式。

在本练习中，你将：

- 检查现有的无障碍自定义智能体。
- 在 Copilot Chat 智能体模式中使用无障碍智能体实现高对比度模式。

无障碍至关重要，因此你将使用自定义智能体尽快生成此功能。

## 检查无障碍自定义智能体

项目已经为无障碍任务创建了自定义智能体。先检查其内容，了解它将如何引导 Copilot。

返回 Codespace，在终端中从 `main` 创建新的分支，以便将无障碍工作与练习 3 的筛选 PR 分开：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 浏览后续部分，了解其中的内容：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的自定义智能体。选择自定义智能体后，该聊天会话就会遵循其指令和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段新对话。每项练习使用的模式和模型可能不同，相关步骤会说明具体设置。

1. 如果 Chat 视图的智能体下拉菜单中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉菜单。默认情况下，当前智能体显示为 **default**。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作，探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中以内联方式显示，并带有文件路径和差异供你审查。
6. 智能体修改文件时，**Files changed** 指示器会更新，显示已更改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会修改 Astro 组件、CSS 和相关测试等文件。

下一练习将审查并指导正在进行的工作。

## 总结和后续步骤

本课程探索了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是专为特定任务和领域定制的 AI 助手。自定义智能体可以将团队的专业知识和标准整理为可复用的智能体，引导 Copilot 更有效地完成特定类型的工作。

你探索了以下概念：

- 如何定义自定义智能体。
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来[监控并指导智能体的工作][next-lesson]，在更改生成时进行审查，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

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
