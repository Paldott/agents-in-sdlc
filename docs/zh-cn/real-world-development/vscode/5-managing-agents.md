---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

在智能体模式中，GitHub Copilot 会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作会在你的计算机上实时进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以直接查看每处差异、接受或拒绝单项更改，并通过后续提示调整或扩展工作，无需离开编辑器。

在本练习中，你将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行时指导智能体，在高对比度模式旁添加浅色模式切换。

## 场景

[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行时，设计团队提出了新需求：还需要浅色模式切换，让觉得默认主题太暗的用户可以使用。你将继续指导现有会话，而不是从头开始新对话。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每次文件编辑都会以内联差异的形式显示，并附带 **Keep** / **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天内容，查看工具调用。你会看到文件读取、文件编辑和请求运行的终端命令。
3. 智能体提出文件编辑时，差异会以内联方式显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以选择聊天顶部的 **Files changed** 指示器，查看此会话中智能体修改的所有文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你愿意运行的命令。

5. 等待智能体完成高对比度模式实现。完成后会显示一条总结更改的消息。

## 指导智能体添加浅色模式

高对比度模式完成后，继续在同一对话中添加浅色模式切换。由于会话没有中断，智能体会保留之前工作的上下文，包括修改的组件、使用的样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续工作，先探索已经做出的更改，再提出浅色模式修改方案。
3. 与审查高对比度模式时一样，逐项检查新编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体的方案不符合预期（例如以不喜欢的方式重构切换 UI），可以选择聊天输入框旁的 **Stop** 按钮停止工作，然后发送澄清提示。

> [!TIP]
> 继续指导正在进行的会话，比从头开始更快。可以通过后续提示要求调整（“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个切换控件，请将浅色模式集成到现有设置面板”）或扩展范围（“为两个切换控件添加键盘快捷方式”）。

## 审查工作区

提交前，快速审查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查已更改文件的完整列表。你应能看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改的文件并查看差异。确认实现采用了自定义智能体要求的无障碍模式，例如无障碍富互联网应用（ARIA）属性、键盘导航、语义化 HTML 和本地存储。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 智能体模式监控并指导正在进行的会话：查看每项文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

在本课程中，你探索了：

- 实时监控智能体的工作。
- 接受和拒绝单项文件编辑。
- 使用后续提示在会话中途指导智能体。

下一课将把无障碍工作发布为拉取请求，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
