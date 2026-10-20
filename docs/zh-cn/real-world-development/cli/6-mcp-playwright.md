---
title: "课程 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它通过浏览器探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是写代码。开发时还需要处理数据、外部服务，并为 Copilot 提供更多自动化能力。模型上下文协议（MCP）服务器可以让 Copilot 使用 CLI 内置功能之外的工具和服务。

在本课程中，你将：

- 了解模型上下文协议（MCP）以及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加该服务器。
- 请智能体驱动浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但验证 UI 更新也需要像用户一样实际操作界面。你将允许 Copilot 使用正在开发的网站，以用户的方式进行交互，从而自动化验证过程，并提高对更改是否符合预期的信心。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时使用外部工具和服务，从而访问最新信息并代表你执行操作。

MCP 服务器负责连接 AI 智能体与外部工具和服务。每个 MCP 服务器都提供一组智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP 服务器][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP 服务器][playwright-mcp-server]**：通过 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助用户发现并为生态系统做出贡献。

> [!CAUTION]
> 将 MCP 服务器视为项目中的其他依赖项。使用前先审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未配置 Playwright MCP 服务器，请添加它，让 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux），保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 使用 Playwright 让 Copilot 探索功能

之前你已手动确认功能是否正常。现在使用 Copilot 中配置的 Playwright MCP 服务器执行同样的验证。

1. 使用以下提示，请 Copilot 调用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自行判断是否使用 MCP 服务器，无需明确指定。不过，指出要用的服务器有助于让结果更一致，也能节省一些令牌。

2. 观察 Copilot 列出它在浏览器中执行的操作，并确认功能。
3. 阅读报告，确认功能行为符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，并向你报告结果。

## 总结和后续步骤

恭喜你，已使用 Playwright MCP 服务器从 Copilot CLI 操作真实浏览器并探索功能。回顾一下，你：

- 了解了模型上下文协议（MCP）以及 Copilot CLI 如何使用它。
- 在尚未配置 Playwright MCP 服务器时添加了该服务器。
- 请智能体驱动浏览器并探索筛选功能。

接下来，创建一个[质量保证（QA）自定义智能体][next-lesson]，将技能和浏览器工具整合到专用角色中。

## 资源

- [什么是 MCP，为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [将 MCP 服务器添加到 Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
