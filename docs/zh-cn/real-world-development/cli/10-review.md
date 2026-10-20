---
title: "课程 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义配置和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装了 Copilot CLI、探索项目并找到预置的筛选议题。
- 添加了星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中构建，并在 Interactive 模式中审查。
- 使用自定义指令引导智能体、自定义现有的 `quality-checks` 技能，并用它运行单元测试、Lint 和类型检查。
- 添加 Playwright 模型上下文协议（MCP）服务器，并用它在真实浏览器中探索筛选功能。
- 创建并选择质量保证（QA）自定义智能体，评估需求、测试覆盖范围、技能结果和浏览器证据。
- 审查了完整的筛选功能更改，并授权 Agent Merge 处理筛选 PR。
- 探索了用于管理上下文、模型、共享和可选云委派的斜杠命令。

## 完成的工作

本研讨会包含两个 PR 里程碑，每个都从更新后的 `main` 分支创建自己的分支：

1. **星级评分**：在游戏卡片上显示已有的 `starRating`，并显示明确的未评分状态。
2. **筛选功能和质量工作流**：实现筛选功能、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建其 PR，你始终使用同一个对话和分支。为简化研讨会流程，这些工作被合并到一个 PR 中。

## 不同类型的验证

你使用多种方式检查代码：自动化测试、手动浏览器检查，以及 Copilot 通过 MCP 在浏览器中探索。`quality-checks` 技能运行了单元测试、Lint 和类型检查，并按照新格式报告结果。QA 在 PR 创建前，将这些结果与需求和测试覆盖审查结合起来。

新增测试应填补实际的覆盖缺口。如果 QA 运行后认为不需要新增测试，也可能是合理结果。授权合并前，先审查代码和证据；代码更改后，请重新运行受影响的验证。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。这些自定义内容可以在多个对话中复用，并根据团队需求不断调整。指令用于设定规范，技能描述可重复的任务，自定义智能体定义专用角色，MCP 服务器则连接外部工具。审查实际更改和工具结果，不要只看智能体总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑实现方案；使用 **Interactive** 参与范围明确的更改；使用 **Autopilot** 处理范围清楚的任务。日常编辑可以选择速度较快的模型，复杂工作则选择能力更强的模型。

上下文与基础设施同样重要。清楚说明希望构建**什么**、**为什么**以及**如何**构建，会显著影响结果。

## 更多探索

核心工作流已经完成。还可以了解以下 CLI 功能：

- `/review`：请代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得不同视角。
- `/fleet`：协调并行执行的独立子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

熟练使用任何工具的最佳方式就是持续实践！可以用它编写生产代码、兴趣项目，或多年来一直想构建的小应用。与团队分享经验，也向团队成员学习。还可以继续探索相关文档。

如果想在终端中扩展 Tailspin Toys，可继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，可探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
