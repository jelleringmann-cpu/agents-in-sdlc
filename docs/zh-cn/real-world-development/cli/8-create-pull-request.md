---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置和测试都保存在同一个分支中。现在该一并审查这些内容并创建拉取请求了。星级评分拉取请求 (PR) 是由你手动合并的；这次将交由 **Agent Merge** 管理流程。

> [!NOTE]
> 通常会把功能、指令更新、技能更新和 QA 智能体拆分成几个独立的 PR。为简化本工作坊，我们将整个筛选和质量工作流保留在同一段对话和同一个分支中，并将所有工作纳入此 PR。

本课将介绍如何：

- 了解 Agent Merge 及其如何自动处理合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证了功能。现在，Tailspin Toys 希望自动完成剩余的 PR 工作，同时由开发人员掌握合并授权。

## Agent Merge 简介

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的事项，例如修复失败的持续集成 (CI) 检查、回复审查意见，以及在需要时执行变基；随后，在存储库允许的情况下启用 GitHub 自动合并。

此前一直是你亲自选择 **Merge pull request**。Agent Merge 可以代为处理，但启用前仍应审查工作并确认 PR 已准备就绪。

## 使用 Agent Merge 管理 PR

代码都已完成，现在一并审查并创建 PR，再让 Agent Merge 管理其余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 在选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 激活 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查通过后，它会执行合并。
7. 确认 Copilot 显示了类似 `"PR #14 was squash-merged successfully."` 的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限不足。如果流程受阻，请查看报告的原因；在存储库允许的情况下，完成审查后手动合并。

## 总结与后续步骤

你自动处理了开发流程中的多个环节，包括生成代码、测试和验证代码，以及管理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动处理合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

接下来，你将[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，了解上下文、模型、共享以及可选的 cloud agent 委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
