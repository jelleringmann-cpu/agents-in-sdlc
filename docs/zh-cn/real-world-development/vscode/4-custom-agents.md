---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]让你能创建专门针对开发工作流中某类任务或领域的 AI 助手。通过在存储库的 `.github/agents` 文件夹中创建 Markdown 文件来定义智能体，可以为 Copilot 提供有针对性的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以把专业经验整理为可复用的智能体，例如执行 [WCAG][wcag] 合规检查的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹或全局 `~/.copilot/agents` 中的 Markdown 文件定义。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description`，后面则是定义智能体行为、专长和指令的 Markdown 提示词。

### 自定义智能体与智能体技能的比较

自定义智能体和[智能体技能][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体负责工作，技能是工具**。

自定义智能体拥有自己的上下文窗口，并在工作时负责协调技能（甚至其他智能体）。在本练习中，无障碍自定义智能体会根据无障碍准则审查并更新网站；作为工作的一部分，它可以调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的正确方法。与 AI 相关的其他工作一样，应根据环境和场景不断测试和改进。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视力能力或偏好的影响。近期用户反馈指出，文本与背景色对比不足，导致一些用户难以阅读当前的深色主题。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。你将使用自定义智能体来生成实现。

本练习将介绍如何：

- 审查现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 审查无障碍自定义智能体

系统已为你创建了负责无障碍工作的自定义智能体。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后打开终端，从 `main` 创建无障碍功能分支。这样可以让练习 3 中的筛选 PR 保持独立：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 接着浏览后续部分，了解其中介绍的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 示例。
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 下定义的所有自定义智能体。选择自定义智能体后，就可以启动遵循该智能体指令并使用其工具的聊天会话。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动全新对话。不同练习所需的模式和模型可能不同，相关步骤会注明。

1. 如果尚未选中，请在 Chat 视图的智能体下拉列表中选择 **Agent**。

   ![Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表。默认情况下，它会显示当前活动的智能体 **default**。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示词：

   ```plaintext
   为网站添加高对比度模式。应有一个可由用户开启或关闭的高对比度开关，并使用浏览器的 local storage 保存设置，以便重新加载页面后仍然生效。
   ```

5. Copilot Chat 会开始工作：探索代码库、提出更改建议并编辑项目文件。每次编辑都会在聊天中显示文件路径和差异，供你审查。
6. 随着编辑内容加入，**Files changed** 指示器会更新，显示智能体已修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一练习中，你将审查并引导这项正在进行的工作。

## 总结与后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是专门针对特定任务和领域设计的 AI 助手。通过自定义智能体，可以将团队专业知识和标准整理为可复用的形式，从而更有效地引导 Copilot 完成特定工作。

本课中，你探索了：

- 如何定义自定义智能体。
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，你将[监控并指导智能体的工作][next-lesson]，审查实时更改，并在同一会话中添加浅色模式开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
