---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前两项练习中，你让 Copilot 云智能体处理了三项任务，目的是改进用户体验并添加功能。虽然云智能体可以异步自主工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，例如 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，你可以查看所有具有开放拉取请求（PR）的智能体任务、探索它们执行的操作，并在会话进行时提供指导。

在本课程中，你将：

- 探索智能体页面，监控云智能体任务。
- 指导正在进行的会话，请求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以同时添加浅色模式。由于网站样式和切换功能已经在更新，加入这一功能很合理。现在你要指导智能体，让它在实现高对比度模式时也添加浅色模式。

## 审查 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的所有任务当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。应该能看到已分配给 Copilot 的任务，包括：
   - 更新代码库文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择正在运行的任务之一，查看 Copilot 执行的任务，其中可能包括：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的各种步骤。

> [!NOTE]
> 具体显示的步骤会根据 Copilot 的工作状态和采用的方法而有所不同。

4. 同时查看右侧显示的拉取请求（PR）窗格，以便监控 PR 和已更改的文件。

## 指导云智能体

现在已了解当前活动任务，接下来请 Copilot 在实现高对比度模式时也添加浅色模式。

1. 选择与添加高对比度模式相关的会话。标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成设置并开始工作。开始出现类似示例中的消息时，表示已开始处理任务。
3. 在 **Steer active session while Copilot is working** 对话框中添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其加入工作流程。

## 等待 Copilot 完成工作

与之前一样，Copilot 会处理更新后的任务。它会先完成收到提示时正在执行的步骤，然后再把新请求纳入工作流程。

这同样需要几分钟。可以利用这段时间回顾之前学过和探索过的内容。

## 总结和后续步骤

本课程探索了 Copilot 智能体页面。它是监控和指导 GitHub Copilot 云智能体任务的中心位置。你可以跟踪所有正在进行和已完成的任务、审查正在执行的工作，并调整任务范围或提供更多指导。

你探索了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来审查 Copilot 创建的[拉取请求并迭代改进工作][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
