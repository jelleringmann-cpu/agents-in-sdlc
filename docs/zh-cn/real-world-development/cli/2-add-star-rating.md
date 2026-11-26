---
title: "第 2 课 - 添加星级评分：快速上手"
description: "使用 Copilot CLI 对游戏卡片进行小幅修改，在转发的浏览器中检查结果，并通过第一个拉取请求合并更改。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以首次修改项目了。我们先从小处着手：游戏数据中已有星级评分，但主页上的游戏卡片还没有显示它。你将要求智能体显示评分、审查更改，并通过第一个拉取请求合并更改。

本课将介绍如何：

- 在功能分支上启动专注于当前任务的 Copilot 对话。
- 要求智能体对项目进行小幅修改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，游戏详情页已经显示该评分。但主页上的游戏卡片只显示标题、类别、发行商和说明。作为热身，你将让智能体在每张卡片上显示现有评分。这项小而独立的修改很适合作为首次会话任务。

## 对话的组成

**对话**是你使用 Copilot CLI 执行任务的地方。与 Copilot app 不同，普通的 CLI 对话使用终端当前检出的存储库和 Git 分支，而不会创建专用工作树。保存的对话可以稍后继续；文件和分支仍是磁盘上的普通 Git 状态。

对话中包含三类内容：你的提示词和智能体的回复、智能体探索和编辑文件时的工具活动，以及可通过 `/diff` 检查的更改。

## 启动对话并请求更改

先启动新对话，再开始实现功能。

1. 返回 Codespace。
2. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未启动，请运行以下命令：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new`，然后按 <kbd>Enter</kbd>，确保已启动新会话。
5. 使用以下提示词请求更改：

   ```plaintext
   在列表页的游戏卡片上显示每款游戏的 starRating（满分 5 分）。如果评分为 null，则显示 "No rating yet"。保持卡片布局不变，添加测试，并运行相关检查。
   ```

Copilot 会探索项目，找到用于显示游戏详情的文件，并编写所需代码。你已使用 Copilot CLI 添加了一项新功能。

## 审查差异

所有 AI 生成的更改在合并前都应经过审查，即使是小改动也不例外。现在直接在 Copilot CLI 中检查更改。

1. 输入 `/diff` 并检查所有已更改的文件。
2. 确认游戏卡片在有评分时显示数值，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖了这两种状态。
4. 查看 Copilot 运行的检查结果；如有失败，请要求它修复。
5. 完成审查后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的工作具有概率性而非确定性，因此实际代码可能有所不同。应审查行为，而不是期待某种特定实现。

## 检查更改

不要只看代码就假定它能正常工作。让 Copilot 启动网站，以便在 Codespaces 转发的浏览器中检查更新后的用户界面 (UI)。

1. 使用以下提示词让 Copilot 启动应用：

   ```plaintext
   启动应用，让我在浏览器中检查星级评分更改。告诉我 URL，并让服务器继续运行。
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示的评分以五分为满分。
4. 返回 Copilot，并使用以下提示词要求它停止自己启动的服务器：

   ```plaintext
   停止你启动的开发服务器。
   ```

## 打开并合并第一个拉取请求

功能已完成，现在创建拉取请求 (PR)，将新代码合并到项目中。

1. 要求默认智能体提交更改：

   ```plaintext
   使用合适的提交消息提交已审查的星级评分更改。
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL 以打开 PR。
4. 审查更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，并使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结与后续步骤

恭喜！你使用 GitHub Copilot CLI 交付了第一个更改。本课中，你：

- 在功能分支上启动了专注于当前任务的 Copilot 对话。
- 指示智能体小幅修改游戏卡片。
- 使用 `/diff` 审查了更改。
- 运行应用，并在转发的浏览器中确认了星级评分。
- 创建并合并了第一个拉取请求。

接下来，你将[从筛选议题开始，使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
