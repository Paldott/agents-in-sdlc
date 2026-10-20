---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或 Copilot 需要了解背景信息，就要确保它能获取这些内容。[指令文件][instruction-files]可以提供指导，让 Copilot 不仅了解要完成什么，也知道应该如何完成。

在本练习中，你将：

- 探索项目专属上下文、编码指南和文档规范如何通过存储库自定义指令及路径范围指令文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级规范。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，本环境不会使用提示展示添加指令前后的变化。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示影响。后续审查云智能体创建的拉取请求时，可以观察指令文件的作用。

## 指令文件

### 场景

Tailspin Toys 团队制定了开发规范和要求：

- 数据层始终需要单元测试。
- UI 应使用深色模式，并具有现代感。
- 代码中的文档应使用 TypeScript 文档（TSDoc）注释。
- 每个文件开头都应添加一段注释，说明该文件的用途。

你将使用指令文件，确保 Copilot 按照这些规范完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解代码风格和要求。你可以指定编码约定、库，以及希望添加的注释类型。指令可以应用于整个存储库，也可以针对特定文件类型提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：适用于存储库中每个请求的单个指令文件。应包含项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可用于特定任务或文件类型，例如特定语言、UI 组件或单元测试。

> [!NOTE]
> 在集成开发环境（IDE）中，指令文件仅用于 Copilot Chat 代码生成，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

本研讨会不会深入讲解如何创建指令文件，但示例项目展示了一种常见做法：

- 将 `copilot-instructions.md` 中的指令限定为项目级指导，例如项目简介、结构和全局编码规范。
- 使用 `*.instructions.md` 为特定文件类型（例如单元测试、Astro 组件和数据层）或任务提供指导。
- 使用自然语言，保持说明清晰，并提供代码示例，展示推荐和不推荐的写法。

创建指令文件没有唯一方法，使用 AI 也没有唯一方法。可以通过试验找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应建立完善的指令文件集合。浏览本项目的文件时，可以看到用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的指令。
>
> Copilot 也可以协助生成指令文件。不同界面中的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前界面的课程会在相关位置说明操作。
>
> 如需模板或起点，可以探索包含指令文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot] 存储库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

先阅读存储库提供的指令文件，其中包括一个核心 `copilot-instructions.md` 和多个用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网站 UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 中的 **GitHub Actions Workflows** 子项。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹浏览内容。这里包含 Astro、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它定义了相对于存储库根目录的 glob 模式，决定哪些文件适用。例如，符合 `**/*.test.ts` 的 TypeScript 测试文件会匹配此模式。
5. 查看该文件中针对项目单元测试的具体说明。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和现有项目文件的链接。这样可以将大型指令拆分为可复用的小文件，并提供项目示例供 Copilot 参考。这里的路径相对于指令文件所在位置。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但还没有规定代码内文档。接下来将添加 TSDoc 注释和文件注释头规则。

## 添加存储库级规范

接下来只需进行一项编辑：添加项目级规则，要求在代码中使用 TSDoc 文档注释和文件注释头。云智能体稍后处理你分配的议题时会读取这项规范。

先创建工作分支（云智能体会从议题目标分支读取指令，但仍应整洁地提交本次更改）：

1. 在 Codespace 终端创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来添加编码规范，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（通常接近第 27 行）。注意，该部分记录了项目编码规范，但尚未要求代码内文档，这就是生成的辅助函数没有文档注释的原因。
3. 在现有规范下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件可以放在存储库级别（`.github/copilot-instructions.md`），提供全局指导；也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指导。存储库级文件适合记录刚添加的项目级文档注释规范。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习中，将议题分配给 Copilot 时，它会从 `main` 分支创建分支。因此，指令更新必须先合并到 `main`，云智能体才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求，并将其合并。下一练习中，云智能体处理议题时就会读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 分支上完成本次研讨会，可以跳过分支步骤并直接提交到 `main`。创建分支是为了让研讨会流程更接近真实项目。

## 总结和后续步骤

你探索了 Copilot 如何读取项目中的指令文件，并在 `.github/copilot-instructions.md` 中添加了存储库级规范。后续审查云智能体创建的 PR 时，可以观察这项规范的作用。

接下来，将第一个议题[分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
