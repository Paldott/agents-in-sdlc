---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或 Copilot 需要了解背景信息，就要确保它能获取这些内容。[指令文件][instruction-files]可以向 Copilot 提供关于代码生成的指导。

在本练习中，你将：

- 探索项目专属上下文、编码指南和文档规范如何通过存储库自定义指令及路径范围指令文件传递给 Copilot。
- 在当前指令下生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。
- 重新发送相同提示，观察生成的代码如何遵循新规范。
- 将指令更新和筛选功能切片提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能与部分规范不一致。Copilot 具有非确定性。本练习的重点是观察更新指令后行为的变化趋势，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 团队制定了一套开发规范和要求：

- 数据层始终需要单元测试。
- UI 应使用深色模式，并具有现代感。
- 代码中的文档应使用 TypeScript 文档（TSDoc）注释。
- 每个文件开头都应添加一段注释，说明该文件的用途。

通过指令文件，可以确保 Copilot 按照这些规范完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解代码风格和要求。你可以指定编码约定、库以及希望添加的注释类型。指令可以应用于整个存储库，也可以针对特定文件类型提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：适用于存储库中每个请求的单个指令文件，应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：可用于特定任务或文件类型，例如特定语言、UI 组件或单元测试。

> [!NOTE]
> 不同环境对其他指令格式的支持有所不同。依赖特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 代码生成，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，会使用存储库级指令文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

示例项目展示了一种常见做法：

- 将 `copilot-instructions.md` 中的指令限定为项目级指导，例如项目简介、结构和全局编码规范。
- 使用 `*.instructions.md` 为特定文件类型（例如单元测试、Astro 组件和数据层）或任务提供指导。
- 使用自然语言，保持说明清晰，并提供代码示例，展示推荐和不推荐的写法。

创建指令文件没有唯一方法，使用 AI 也没有唯一方法。可以通过试验找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应建立完善的指令文件集合。项目中有用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的指令。
>
> Copilot 也可以协助生成指令文件。不同界面的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前界面的课程会在相关位置说明操作。
>
> 如需模板或起点，可以探索包含指令文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot] 存储库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

先阅读入门项目中包含的指令文件，其中包括核心 `copilot-instructions.md` 和多个用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网站 UI 中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 中的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹浏览内容。这里包含 Astro、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它定义了相对于存储库根目录的 glob 模式。例如，符合 `**/*.test.ts` 的 TypeScript 测试文件会匹配该模式。
5. 查看该文件中针对项目单元测试的具体说明。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他指令文件（例如 `unit-tests.instructions.md`）和现有项目文件的链接。这种方式可以将大型指令拆分为可复用的小文件，并提供示例供 Copilot 参考。路径相对于指令文件所在位置。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但还没有规定代码内文档。接下来将添加 TSDoc 注释和文件注释头规则。

## 为更改创建分支

先为本次更改创建分支。

1. 返回上一练习使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一段新对话。每项练习使用的模式和模型可能不同，相关步骤会说明具体设置。

先使用当前指令文件发送提示，观察 Copilot 的结果。稍后更新指令后，再次发送相同提示进行比较。

1. 关闭之前练习留下的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应存放的位置。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件及其测试。
6. 注意，建议的辅助函数是类型化函数，以 `db` 客户端为首个参数，并返回类型化的发行商数组。这些约定来自 `.github/instructions/drizzle.instructions.md`，该文件适用于 `src/lib/*.ts`。
7. 注意，建议的代码**缺少** TSDoc 注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使没有明确要求，也可能添加文档注释。如果出现这种情况也没关系；重点是观察更新指令后行为是否更一致。

## 添加存储库级规范

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码规范，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（通常接近第 27 行）。该部分记录了项目编码规范，但还没有要求代码内文档。
3. 在现有规范下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件可以放在存储库级别（`.github/copilot-instructions.md`），提供全局指导；也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指导。存储库级文件适合记录项目范围的规范。

## 重新发送提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送之前完全相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意，建议的文件现在会以类似以下的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意，建议的函数现在包含类似以下的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目规范，并将其应用于后续练习会继续使用的代码。

## 提交指令并推送分支

指令文件和其他项目文件一样，需要通过源代码管理进行管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端运行以下命令，确认指令更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有文件更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他辅助更改（例如测试），也请暂存这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，创建拉取请求并关联待办事项中的文档规范议题。Copilot 可使用已连接到项目的 GitHub 工具完成这项工作。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。之后的练习会介绍这种连接如何通过模型上下文协议实现。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认选择了 **Agent**。
3. 请 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. Copilot 请求代表你执行 GitHub 操作时，检查命令，并酌情选择 **Allow**。
6. PR 创建后，请 Copilot 合并 PR 并将本地分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求代表你执行 GitHub 操作和 Shell 命令时，检查命令，并酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并了拉取请求。

## 总结和后续步骤

你探索了 Copilot 如何读取项目中的指令文件，并在 VS Code 中使用 Copilot Chat：

- 在现有指令下发送代码生成提示，并观察 Copilot 的输出。
- 在 `.github/copilot-instructions.md` 中添加存储库级规范。
- 重新发送相同提示，观察生成的代码如何遵循新规范。
- 将指令更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，在[智能体模式][next-lesson]中使用这些指令，让 Copilot 跨多个文件实现新功能。

## 资源

- [自定义 GitHub Copilot 指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
