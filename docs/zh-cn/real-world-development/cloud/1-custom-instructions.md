---
title: "练习 1 - 自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应了解某些背景信息，就要确保它能获取这些上下文。[指令文件][instruction-files]用于提供此类指导，帮助 Copilot 理解你希望它做什么以及如何完成。

本练习将介绍如何：

- 探索如何通过存储库自定义指令和路径范围指令文件，将项目上下文、编码指南和文档标准传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的全局存储库标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习不同，这里不会运行前后对比提示词。Copilot cloud agent 会异步处理 GitHub 议题，因此难以实时并排展示效果。稍后审查 cloud agent 创建的拉取请求时，你会看到指令文件的影响。

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

- `.github/copilot-instructions.md`：每次针对存储库的请求都会发送给 Copilot 的单个指令文件。它应包含与大多数请求相关的项目级信息，例如所用技术栈、正在构建的内容、最佳实践和其他全局指导。
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
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定适用的文件。此处会匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看本项目中关于创建单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这可以将较大的指令集拆分成较小、可复用的文件，并提供示例供 Copilot 参考。相关路径是相对于指令文件的，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但尚未要求为代码添加文档。接下来会添加 TSDoc 文档注释和文件头部注释规则。

## 添加新的存储库标准

接下来是本练习唯一需要编辑的内容：添加一项项目级规则，要求在代码中使用 TSDoc 文档注释和文件头部注释。之后的练习中，将议题分配给 cloud agent 时，它会读取这项指令。

编辑前先创建一个分支。（cloud agent 会从议题所指向的分支读取指令，但你仍会单独提交这些更改。）

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到靠近第 27 行的 **Code formatting requirements** 部分。注意其中记录了项目编码标准，但尚无代码文档规则，这也是之前生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown，要求添加文件头部注释和 TSDoc 文档注释：

   ```markdown
   - 每个导出的函数都应有 TSDoc 注释，说明其用途、参数和返回值。
   - 在导入语句或任何代码之前，添加注释块说明文件的用途。
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件既可以放在存储库级文件（`.github/copilot-instructions.md`）中，为整个项目提供指导；也可以放在 `*.instructions.md` 文件中，针对特定语言、文件类型或任务提供指导。像文档注释规则这样的项目级标准适合放在存储库级文件中。

## 提交、推送并合并指令更新

Cloud agent 会读取议题目标分支中的指令文件。下一练习将议题分配给 Copilot 时，它会从 `main` 分支创建新分支，因此必须先将指令更改合并到 `main`，cloud agent 才能读取。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后的练习中，cloud agent 就会读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 上完成本工作坊，可以跳过分支步骤并直接提交到 `main`。这里保留分支步骤，是为了模拟真实项目中的工作方式。

## 总结与后续步骤

你探索了 Copilot 如何读取本项目的指令文件，并向 `.github/copilot-instructions.md` 添加了一项存储库级标准。接下来几节中，你会看到 cloud agent 创建的拉取请求如何体现这项标准。

接下来，将[第一个议题分配给 Copilot cloud agent][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个提示][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
