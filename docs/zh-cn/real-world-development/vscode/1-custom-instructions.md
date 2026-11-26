---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应了解某些背景信息，就要确保它能获取这些上下文。与 Copilot 分享具体上下文的方法有很多，其中最重要的是[指令文件][instruction-files]，它可以为代码生成提供指导。

本练习将介绍如何：

- 探索如何通过存储库自定义指令和路径范围指令文件，将项目上下文、编码指南和文档标准传递给 Copilot。
- 使用当前指令生成筛选功能的第一部分数据逻辑（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的全局存储库标准。
- 再次运行相同提示词，观察生成的代码如何采用新标准。
- 将指令更新和筛选功能提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合部分既定标准。Copilot 的输出具有不确定性。本练习的重点是观察更新指令前后的行为趋势，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践准则和要求：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应有注释块，说明该文件的用途。

使用指令文件，确保 Copilot 获得正确的信息，并按照这些实践完成任务。

### 自定义指令

自定义指令为 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和要求。你可以指定编码约定、库，以及希望代码中采用的注释类型。可以为整个存储库创建指令，也可以针对特定文件类型创建任务级指令。

指令文件分为两类：

- `.github/copilot-instructions.md`：每次针对存储库的请求都会发送给 Copilot 的单个指令文件。它应包含与大多数 Copilot Chat 或 CLI 请求相关的项目级上下文，例如所用技术栈、正在构建的内容、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建。例如，可以为 TypeScript 或 Astro 等语言提供指导，也可以针对创建 UI 组件或一组新单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时，都会使用存储库级指令，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

全面讲解如何编写指令文件超出了本工作坊的范围。示例项目展示了一种有代表性的方法。总体而言：

- 让 `copilot-instructions.md` 专注于项目级指导，例如正在构建的内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，清楚地编写指导，并提供正确和错误的代码示例。

创建指令文件并没有唯一的方法，使用 AI 也是如此。通过实践找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。探索本项目中的指令时，你会看到许多不同任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> 也可以让 Copilot 帮你生成指令文件。不同产品界面的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应产品的课程会介绍相关操作。
>
> 如果需要模板或起点，可以浏览 [awesome-copilot][awesome-copilot]，其中收集了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目中的自定义指令文件

本存储库附带一个核心 `copilot-instructions.md` 文件，以及一组面向不同任务的 `*.instructions.md` 文件。请在编辑器或 GitHub 网页界面中打开并浏览这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定适用的文件。例如，这里会匹配符合 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看本项目中关于创建单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这可以将较大的指令集拆分成较小、可复用的文件，并提供示例供 Copilot 参考。相关路径是相对于指令文件的，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但尚未要求为代码添加文档。接下来会添加 TSDoc 文档注释和文件头部注释规则。

## 为更改创建分支

遵循最佳实践，在独立分支中进行更改。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令*之前*先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动全新对话。不同练习所需的模式和模型可能不同，相关步骤会注明。

若要观察自定义指令的影响，先在当前指令下发送提示词。稍后更新指令后，再发送相同提示词以比较结果。

1. 关闭前面练习留下的编辑器选项卡，确保 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，让 Copilot 能应用文件更改。

   ![Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发行商的名称和 ID。应用文件更改。
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件及其测试。
6. 注意建议的辅助函数是一个类型化函数，它将 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件级头部注释。

> [!CAUTION]
> Copilot 的输出具有不确定性，即使没有明确要求，也可能添加文档注释。如果发生这种情况也没关系，本练习仍可展示更新指令后输出一致性的改善。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一项存储库编码标准，改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到靠近第 27 行的 **Code formatting requirements** 部分。注意其中记录了项目的编码标准，但尚无代码文档规则，这也是之前生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown，要求添加文件头部注释和 TSDoc 文档注释：

   ```markdown
   - 每个导出的函数都应有 TSDoc 注释，说明其用途、参数和返回值。
   - 在导入语句或任何代码之前，添加注释块说明文件的用途。
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件既可以放在存储库级文件（`.github/copilot-instructions.md`）中，为整个项目提供指导；也可以放在 `*.instructions.md` 文件中，针对特定语言、文件类型或任务提供指导。像文档注释规则这样的项目级标准适合放在存储库级文件中。

## 再次运行提示词并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清除对话内容。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦正确的文件。
3. 发送与之前**相同的提示词**：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发行商的名称和 ID。应用文件更改。
   ```

4. 注意建议的文件现在以类似下面的注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 注意建议的函数现在包含类似下面的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行商及其 ID 和名称。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为发行商对象数组。
    */
   ```

你通过添加新的项目标准引导了 Copilot，并将其应用到下一练习会继续使用的实际代码中。

## 提交指令并推送分支

指令文件和其他资源一样，也需要使用源代码管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在终端中打开新窗口。
2. 在终端中运行以下命令，确认指令和辅助函数更改已保存：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他配套更改（例如测试），也将这些文件添加进来。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求，并关联待办列表中的文档标准议题。也可以手动完成，但不妨让 Copilot 使用已连接的 GitHub 工具查找议题、创建用于关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已连接 GitHub 工具，无需另行设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。请按照提示操作。之后的练习会介绍这种连接所用的模型上下文协议 (MCP)。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中的是 **Agent**。
3. 要求 Copilot 查找与指令文件更新有关的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础：

   ```
   查找与更新指令文件有关的议题。从当前分支创建一个新 PR，说明该 PR 会关闭此议题，并注明我们还添加了供后续筛选工作使用的发行商辅助函数基础。
   ```

4. Copilot 会开始查找议题并创建 PR。
5. Copilot 请求 **Allow** 以代你执行 GitHub 操作时，请先审查命令，再决定是否允许。
6. PR 创建后，使用以下提示词要求 Copilot 将其合并到 `main` 并在本地切回主分支：

   ```
   将 PR 合并到 main。然后在本地切回 main 并拉取最新代码，确保我们已同步到最新状态。
   ```

7. Copilot 请求 **Allow** 以代你执行 GitHub 操作和 shell 命令时，请先审查命令，再决定是否允许。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结与后续步骤

你探索了 Copilot 如何从指令文件获取上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示词，观察现有指令下 Copilot 的输出。
- 向 `.github/copilot-instructions.md` 添加新的全局存储库标准。
- 再次发送相同提示词，观察代码如何采用新标准。
- 将指令更新和辅助函数基础提交到 `main`，以便下一练习继续使用。

接下来，在[智能体模式][next-lesson]中运用这些指令，让 Copilot 跨多个文件实现新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个提示][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
