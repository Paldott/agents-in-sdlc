---
title: "课程 8 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置文件和测试都已保存在同一分支。现在一起审查这些内容并创建拉取请求。本次将使用 **Agent Merge** 管理流程，而不是像星级评分 PR 那样手动合并。

> [!NOTE]
> 在实际项目中，通常会把功能、指令更新、技能更新和 QA 智能体拆分为几个独立 PR。为简化研讨会流程，这些筛选和质量工作都在同一对话和分支中完成，并放入同一个 PR。

在本课程中，你将：

- 了解 Agent Merge 及其如何自动化 PR 合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能工作流中，你已使用 Copilot 规划、实现并验证功能。现在 Tailspin Toys 希望自动完成 PR 的后续工作，同时由开发者保留合并授权权。

## 介绍 Agent Merge

**Agent Merge** 会自动处理 PR 合并前的剩余工作。启用后，Copilot 会处理阻止 PR 合并的问题，例如修复失败的持续集成（CI）检查、响应审查意见，以及在需要时执行变基；在存储库允许的情况下，之后会启用 GitHub 自动合并。

此前一直由你手动选择 **Merge pull request**。Agent Merge 可以接手这项工作，但启用前仍要先审查更改并确认已准备就绪。

## 使用 Agent Merge 管理 PR

所有代码都已完成，接下来一起审查，创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或缺失的权限。如果流程受阻，请阅读报告的原因，并在存储库允许的情况下手动完成已审查的合并。

## 总结和后续步骤

你已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及管理 PR 流程。你：

- 了解了 Agent Merge 及其如何自动化 PR 合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，探索[更多实用的 Copilot CLI 斜杠命令][next-lesson]，了解上下文、模型、共享和可选的云委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
