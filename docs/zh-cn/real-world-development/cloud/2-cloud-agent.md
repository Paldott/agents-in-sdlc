---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

许多组织都会面对技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿拉取请求（PR），供开发者审查。这样可以把繁琐任务委派出去，加快开发流程，让开发者专注于更重要的工作。

在本课程中，你将使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保操作安全。
- 了解清晰界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初负责创建网站的承包商没有留下文档。首先，团队希望为应用程序中的所有导出函数添加 TypeScript 文档（TSDoc）注释。

此外，设计团队希望改进游戏发现体验。他们希望每款游戏的详情页都能显示相关游戏，也就是同一类别中的其他游戏。现阶段不需要完善的实现，只需能用于验收测试用户体验（UX）。这项工作目前阻碍了进度，但其他议题优先级更高。

这类容易被降级处理的任务适合交给 Copilot 云智能体异步处理。开发者可以先做其他工作，之后再回来审查 Copilot 的结果。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。例如，可以[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在会话执行期间或完成后查看它的工作。解决方案准备好审查时，Copilot 会在 PR 中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 并非魔法，使用它并不意味着可以跳过思考。即使看似简单的操作，深入分析后也可能很复杂，因此请注意[如何界定 Copilot 云智能体的任务范围][cloud-agent-best-practices]。将它视为 AI 结对编程伙伴：分阶段工作、学习、试验并逐步调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果已完成自定义指令练习，就已了解 `.github/copilot-instructions.md` 的效果；否则现在可以快速查看。将工作分配给云智能体前，先只读查看存储库中的指令文件，之后便于观察它们的影响。

在 GitHub 网站 UI 或已启动的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释的要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，了解这些指令如何适用于 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意 PR 中的 TSDoc 注释、TypeScript 约定和注释头；这些要求来自指令文件。之后审查 PR 时还会再次说明。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要设置特定环境和脚本。将任务分配给 Copilot 时也一样，因为它会像软件工程师（SWE）一样执行任务。

云智能体使用 [GitHub Actions][github-actions] 作为工作环境。可以通过 `.github/workflows/copilot-setup-steps.yml` 文件配置[专用设置工作流][setup-workflow]，在它开始工作前运行，以便访问所需的开发工具和依赖项。本实验已预先配置该工作流，方便体验流程。它会为这个单体 Astro 应用提供 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及完成迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的 "Actions" 选项卡中手动测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果不在设置步骤中克隆，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

这个文件与其他 GitHub 工作流类似，但有几个值得注意的地方：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会执行该作业。
- `workflow_dispatch` 触发器可让你在存储库的 **Actions** 选项卡中手动运行工作流。这样可以测试工作流是否成功，无需等待 Copilot 执行。

## 添加文档

大多数项目都存在信息过期或缺少文档的问题。这类技术债务往往长期未解决，降低工作效率，也让维护代码库或让新开发者加入团队变得更困难。Copilot 擅长生成文档，因此可以将这项工作分配给云智能体，让它在后台生成所需文档。之后的练习会审查其工作。

1. 在新浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的 PR，标题类似 `[WIP]: Code lacks documentation`。如果列表中还没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表。它正在创建环境（运行前文介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看待办事项以及 Copilot 将要完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 查看实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并制定适当计划，以及创建代码。

这可能需要几分钟。云智能体的主要优势之一是可以异步执行任务，让开发者专注于其他工作。接下来再分配一个任务给云智能体，同时继续为应用添加功能。

## 为游戏详情页添加相关游戏

GitHub Copilot 云智能体的一大优势是可以拆分工作：你处理一项任务，Copilot 同时处理另一项任务。即使为设计团队添加相关游戏区域不会花费太久，也可以把这段时间用于其他工作。现在将任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，议题的第一条评论中会出现一组 👀，表示 Copilot 正在处理任务。

![Copilot 使用眼睛表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。云智能体的工作方式类似软件工程师，因此无需持续监控；等它完成后再审查即可。现在可以继续创建并使用自定义智能体。

## 总结和后续步骤

本课程探索了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给云智能体，让它异步处理技术债务、新功能或代码框架迁移任务。

你探索了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 清晰界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以开始创建并使用自定义智能体。[Copilot 云智能体也可以使用模型上下文协议（MCP）服务器][cloud-agent-mcp]，并支持前面练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
