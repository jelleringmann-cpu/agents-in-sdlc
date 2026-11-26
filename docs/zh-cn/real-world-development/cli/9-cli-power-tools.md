---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话，以及可选委派给 cloud agent 的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多可用于交互的斜杠命令。这些命令提供高级功能、幕后信息和额外配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在一起来看看其他实用命令。

本课将介绍如何：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树以及委派给 cloud agent 的可选命令。

## 场景

CLI 核心工作流已经完成。现在了解一些额外功能：管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot cloud agent][about-cloud-agent]。

## 查看 Copilot CLI 上下文

执行大型或复杂任务时，可能会遇到模型的最大上下文窗口限制。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令自行查看或压缩上下文。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及系统指令、工具、消息和剩余空间所占的上下文。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近填满时，Copilot CLI 会自动压缩上下文。使用 `/compact` 可以自行选择何时压缩。若要切换到无关任务并开始全新对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发人员也有不同偏好。Copilot CLI 可以列出可用模型，并让你选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd>关闭列表。

## 共享会话

团队合作并分享经验有助于大家更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享的链接，或发布为 GitHub gist。

1. 输入 `/help`，查看当前安装版本中 `/share` 的选项。
2. 如果要共享此会话，请选择合适的目标，例如 `/share file` 可导出本地 Markdown 文件。
3. 将导出内容发送给他人或发布之前，先审查内容。会话导出文件可能包含提示词、回复和项目详情。

是否发布链接或 gist 由你决定。不要公开团队不打算分享的存储库或对话内容。

## 可选：扩展并行工作或委派任务

核心工作坊已经结束。Copilot CLI 还提供了一些适用于大型任务的命令：

- `/fleet` 可将彼此独立的子任务分配给子智能体并行执行。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给异步工作的 Copilot cloud agent；它可能会打开拉取请求。

这些命令可能会创建额外工作树或启动远程工作，因此是可选的。尝试前先创建范围明确的新任务，并按常规工作流审查结果。若要进一步了解异步智能体工作，请继续学习 [cloud agent 工作坊][cloud-workshop]。

## 总结与后续步骤

使用 Copilot CLI 的斜杠命令，可以配置工具、共享会话并查看幕后发生的情况。本课中，你：

- 使用 `/context` 和 `/compact` 查看了 Copilot 如何管理对话上下文。
- 使用 `/model` 查看了可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了用于并行工作、工作树和可选委派给 cloud agent 的命令。

Copilot CLI 还有更多命令和功能可供探索！接下来，我们将[回顾所学内容并了解后续步骤][next-lesson]。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
