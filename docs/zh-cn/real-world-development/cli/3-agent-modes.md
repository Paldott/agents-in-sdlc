---
title: "课程 3 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 商定实现方案，使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚为项目添加了一项小功能。更大的更改则需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作流，帮助团队正确地构建所需内容。本课程是多个课程中的第一节，你将遵循典型的智能体辅助开发流程：从议题开始构建功能、确保代码有效且行为符合预期，最后成功合并到项目中。

> [!NOTE]
> 后续功能工作会继续使用同一对话和分支。通常，不同类型的文件会放在不同分支或 PR 中处理；本研讨会为了聚焦核心概念，会简化这一步骤。

在本课程中，你将：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续开发此功能时，你还将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并打开功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访客需要按类别和发行商筛选游戏。待办事项中的议题描述了该功能，但组合筛选类别等细节需要在编码前确定。你将使用 Plan 模式确认这些决策，然后授权 Autopilot 在明确范围内实现功能。

## 背景

在开发流程中加入 AI 编码智能体，不会改变软件开发的基本原则。事实上，这些原则更重要了！大多数开发者遵循的流程大致如下：

1. 从描述需求的已提交议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程与上述步骤相似。

遵循标准流程，可以确保 AI 生成的代码符合既定需求，并接受与人工编写代码相同的审查。

## 对话模式

**对话模式**决定智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作，包括编写代码、运行测试并迭代，直到认为任务已完成。

先在 Plan 模式中审查计划，然后使用 Autopilot 实现计划。

## 从议题开始

开始筛选功能前，先返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重启。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main`，然后创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用箭头键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，并返回 **Session** 选项卡。

此时提示开头会出现 `#7` 或类似编号。`#` 可将 GitHub 上的议题或拉取请求（PR）添加到上下文中。

## 规划筛选功能

规划有助于在开始实现功能或执行任务前明确方案。处理复杂任务时，花些时间规划通常很有帮助。现在切换到 Plan 模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用之后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划：先探索项目，再确定合适的实现方案。

3. 过程中 Copilot 可能会询问筛选行为的细节。根据你的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 浏览计划内容。
6. 如果计划与已确定的选择不符，请要求 Copilot 修改。

## 批准 Autopilot

计划已编写并审查完毕，接下来开始实现。使用 Autopilot 让 Copilot 执行计划。

Autopilot 会让 Copilot 反复处理问题，直到它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或已安装版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按已确定的计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此务必先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再将检查视为完成。

## 审查并验证实现

生成的代码与其他代码一样，在合并前都需要审查。现在检查代码并运行网站，确认功能正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 对照议题和规划时做出的决定审查结果。
4. 审查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果；如果失败，请要求 Copilot 修复。

## 体验新功能

代码看起来不错，但它能运行吗？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中商定的组合方式。
4. 确认重置筛选和无结果时的行为符合议题和已做出的决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同对话模式构建并审查功能。在本课程中，你：

- 从 GitHub 议题启动了新的 Copilot 对话。
- 在 Plan 模式中明确了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来通过[使用自定义指令][next-lesson]深入了解代码生成，确保生成的代码遵循文档化规范。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
