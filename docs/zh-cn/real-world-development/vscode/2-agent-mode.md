---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。作为开发人员，你的工作流程可能包括：查找所有相关文件、进行修改、运行测试、调试、发现遗漏的文件、再次修改……还有很多类似步骤。

Copilot 智能体模式可以帮助处理这些工作。

Copilot 智能体模式旨在让 Copilot 在 IDE 中更自主地工作。它会像开发人员一样探索现有项目结构、执行必要更新、运行测试等任务，并自动修复发现的问题。接下来了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和 cloud agent 名称相似，但适用于不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互。Cloud agent 则像团队成员一样通过议题和拉取请求与你协作，并异步完成工作。

本练习将介绍如何：

- 使用 Copilot 智能体模式探索项目、查找相关文件并协调多文件更改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在将代码合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断扩大，你希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行任何更改前，先浏览 Tailspin Toys 网站，了解它目前的功能。

该网站是一个带有开发者主题的桌游众筹平台，用户可以浏览游戏列表和详情。它是一个单独的 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 frontmatter 使用 Drizzle ORM 直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中；交互功能则通过使用标准 DOM API 的小型、限定范围的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行网站：

1. 返回 codespace。上一练习结束后，你应该已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅和 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接时，系统会自动将其转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的专用隧道，网站服务器正在该 Codespace 中运行。

### 浏览网站

网站运行后，可以浏览其功能。主要功能包括：

- **主页**：显示桌游列表，包括名称、图片和说明。
- **游戏详情页**：选择游戏后，会打开详情页并显示更多信息，包括名称、说明、发行商和类别。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动全新对话。不同练习所需的模式和模型可能不同，相关步骤会注明。

网站的初始实现已经可以正常使用，但我们还希望添加新功能。先查看待办列表。使用模板创建项目时，系统已自动创建一组议题。让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。智能体会自主规划并跨多个文件实现更改、运行终端命令并调用工具。

   ![Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非特别偏好某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将其列入说明。这样做是希望代码建议较为一致，从而提供良好的体验。不过，大型语言模型的输出具有概率性，你看到的建议可能与课程说明不同。这完全正常。

> [!NOTE]
> Copilot 可能会因其概率性而使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示词，要求它显示 GitHub 议题待办列表：

   ```plaintext
   请显示我的 GitHub 存储库中的待办事项，并根据对用户最有用的程度帮助我确定优先级。
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 审查生成的议题列表。

注意 Copilot 会根据它认为对用户最有用的程度为项目排序。

## 审查指令文件

让智能体生成代码之前，先查看将用于提供上下文的指令文件。这里将使用[用户界面 (UI) 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能时需要遵循的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用指令的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置。这样便于重复使用内容、拆分较大的指令，以及提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行以下三项修改：

- 在数据层 (`src/lib/`) 中添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，必须在合并到代码库之前运行测试并确认全部通过。Copilot 智能体模式可以完成这些工作。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui**，添加指令文件。

   ![选择 UI 指令文件的示例屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选择了 **Agent**。

   ![Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 作为模型。
5. 使用以下提示词，让 Copilot 根据待办列表中的相关议题实现功能：

   ```plaintext
   请根据待办列表中相关 GitHub 议题的要求，为网站添加按发行商和类别筛选的功能。上一练习中已经有发行商辅助函数，请保留并在需要时完善已有工作，同时完成其余数据层、UI 和测试。确保所有测试通过。服务器已经在运行，不需要重新启动。
   ```

6. 观察 Copilot 如何探索项目并查找相关文件。它应该会找到数据层辅助函数、UI 和测试文件，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 按钮，因此无需在工作过程中立即选择。

7. 按 Copilot 的提示选择 **Continue**，运行测试。

   ![Copilot Chat 窗格中提示用户确认运行测试的对话框](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现暂停，也可能有测试失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直至任务完成且未发现错误。

   ![完整 Copilot Chat 智能体模式会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 无论代码由 Copilot 还是其他 AI 工具生成，都必须审查。

10. 返回正在运行网站的浏览器，试用新功能！
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你了解到：

- 如何使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、查找相关文件并协调多文件更改。
- 如何在将代码合并到代码库之前审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 附加探索练习 - 实现分页

随着游戏列表不断增加，可能需要实现分页。运用本练习中学到的技能，提示 Copilot 为网站添加分页。编写代码时可考虑：

- 遵循现有最佳实践，包括使用已有的指令文件。
- 决定分页方式：允许用户选择每页大小，还是使用固定大小。
- 编写提示词时，提供必要指导，使 Copilot 按照你的预期实现功能。
- 你可能需要与 GitHub Copilot 多次交互，要求修改并提供上下文。这是使用 Copilot 时的正常流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
