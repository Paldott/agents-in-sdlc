---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也要修改多个文件并执行测试等操作。开发流程往往包括查找相关文件、进行修改、运行测试、调试、发现遗漏文件，再次修改……事情很多。

这正是 Copilot 智能体模式的用武之地。

Copilot 智能体模式可以在集成开发环境（IDE）中更自主地工作。它会像开发者一样探索现有项目结构、执行必要更新、运行测试等任务，并自动修复发现的问题。现在了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体虽然名称相似，但适用于不同场景。智能体模式在 IDE 中执行任务，方便快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解：

- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- GitHub Copilot 智能体模式如何跨用户界面（UI）和数据层实现新功能。
- 合并代码前如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断扩大，你希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在使用 Copilot 智能体模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

修改代码前，先浏览 Tailspin Toys 网站，了解当前功能。

该网站是一个以开发者为主题的棋盘游戏众筹平台，可列出游戏并显示详情。它是单体 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有独立的后端应用程序编程接口（API）或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能通过使用标准 DOM API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。在 GitHub Codespace 中按以下步骤操作：

1. 返回 Codespace。上一练习结束后应已切换回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示你打开 `http://localhost:4321` 的对话框，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost URL 时，会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的专用隧道，网站服务器正在其中运行。

### 浏览网站

网站启动后，可以浏览其功能。主要功能包括：

- **首页**：显示棋盘游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，显示游戏标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段新对话。每项练习使用的模式和模型可能不同，相关步骤会说明具体设置。

网站初始版本已可运行，但我们希望添加新功能。先检查待办事项。通过模板设置项目时，系统会自动创建一组议题；请 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。该智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未表示偏好某个模型。编写研讨会时使用的是 Claude Sonnet 4.5，因此在步骤中列出该模型，希望让代码建议相对一致。由于大语言模型（LLM）具有概率性，收到的建议可能与示例不同，这是正常且预期的情况。

> [!NOTE]
> 由于大语言模型具有概率性，Copilot 可能会使用不同的模型上下文协议（MCP）命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示，询问议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 还会根据它认为对用户最有用的程度对议题进行排序。

## 审查指令文件

让智能体生成代码前，先查看将用于提供上下文的指令文件。本练习会使用[用户界面（UI）指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中包含为网站添加功能的指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向 Astro、Tailwind CSS 和 Drizzle 等文件类型专用指令的链接。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置。这样可以复用内容、将较大的指令拆分为小文件，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要更新三个部分：

- 添加或完善数据层（`src/lib/`）中的筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

测试还必须运行并通过，才能将更改合并到代码库。现在让 Copilot 智能体模式执行这些任务。

1. 继续使用当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui**，添加 UI 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选择 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并定位相关文件，包括数据层辅助函数、UI 和测试。然后它会开始修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库复杂度，这可能需要几分钟。在过程中，代码编辑器中可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，会为所有更改提供 **Keep** 或 **Undo** 选项，因此无需在工作进行时选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中要求确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这是正常的。Copilot 会反复生成代码并运行测试，直到完成任务且不再检测到错误。

   ![Copilot 智能体模式完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在问题。

> [!CAUTION]
> 始终要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行的网站，在浏览器中体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜你！本练习探索了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。你了解了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- 合并代码前如何审查 Copilot 智能体模式生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索：实现分页

随着游戏列表不断增长，需要添加分页。使用本练习学到的技能，请 Copilot 更新网站以实现分页。可以考虑以下问题：

- 遵循现有最佳实践，包括使用现有指令文件。
- 决定分页方式，例如允许用户选择每页条目数，或将其固定。
- 编写提示时提供 Copilot 所需的指导，确保实现符合预期。
- 可能需要迭代并提供更多上下文，让 GitHub Copilot 修改结果。这是正常的协作流程。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
