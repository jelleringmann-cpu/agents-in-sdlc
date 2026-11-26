---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索存储库指令，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应向 Copilot 提供相应指导。[指令文件][instruction-files]不仅说明你想要什么代码，也说明代码应如何组织。现在筛选功能已经完成，你将探索 Copilot 使用的指令、添加一项文档标准，并将其应用于代码。

本课将介绍如何：

- 探索存储库指令和路径范围指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 查看指令文件对代码的影响。

## 场景

与优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 记录用途、参数和返回值；如果有可注入的 `db` 参数，也应记录。
- 可复用的 Astro 组件应记录其 `Props` 契约；相关代码更改时，注释也应保持最新。
- 应保留现有格式和 lint 指南。

使用指令文件，可以确保 Copilot 获得正确的信息，并按照这些实践完成任务。

## 指令文件

自定义指令会为 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。你可以指定编码约定、库，以及希望代码中采用的注释类型。可以为整个存储库创建指令，也可以针对特定文件类型创建任务级指令。

指令文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。它应包含与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型提供指导，例如 TypeScript 或 Astro，或者创建 UI 组件或单元测试。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。依赖特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 探索项目中的自定义指令文件

初始项目已包含一组指令文件。修改前先了解现有文件及其作用。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，注意其中的项目简介和编码指导。这些指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了用于确定适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他指令文件和项目现有文件的链接。这有助于将较大的指令集拆分成较小、可复用的文件，并提供示例供 Copilot 参考。

## 更新指令文件以符合团队指南

现有文件是不错的起点，但仍有缺漏。接下来修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含文档用的 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，约在第 35 行。
2. 在该部分的最后添加以下条目：

   ```markdown
   - 所有新建的 TypeScript 代码都应包含用于文档说明的 TSDoc 注释。
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 会在每次启动对话时加载存储库指令。保存更新后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 使用以下提示词要求 Copilot 应用更新后的指导：

   ```plaintext
   我们刚刚更新了指令和代码指导。请将新指导应用到你生成的代码中。
   ```

4. 输入 `/diff` 并查看更改过的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认它们准确描述了代码。

## 总结与后续步骤

你探索了 Copilot 如何从指令文件获取上下文，并将新的存储库标准应用于功能。本课中，你：

- 探索了存储库指令和路径范围指令文件如何传递给智能体。
- 更新了指令文件，确保遵循编码标准。
- 查看了指令文件对代码的影响。

接下来，你将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
