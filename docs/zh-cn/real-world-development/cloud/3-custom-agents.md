---
title: "练习 3 - 自定义智能体"
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

你将通过自定义智能体探索：

- 如何定义自定义智能体。
- 如何为自定义智能体分配任务。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视力能力或偏好的影响。近期用户反馈指出，文本与背景色对比不足，导致一些用户难以阅读当前的深色主题。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。你将使用自定义智能体来生成实现。

## 审查无障碍自定义智能体

系统已为你创建了负责无障碍工作的自定义智能体。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，然后查看无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 接着浏览后续部分，了解其中介绍的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 示例。
   - 常见陷阱和错误。

## 创建并分配议题

控制中心是管理环境中所有智能体的中心位置。你可以将任务分配给 Copilot cloud agent、监控任务并提供后续指导。现在将创建高对比度模式的任务并分配给 Copilot。

1. 转到存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 在 **Title** 中输入 `为网站添加高对比度模式`。
6. 在 **Description** 中输入：

    ```plaintext
    我们需要为网站添加高对比度模式。应提供一个可由用户设置的高对比度开关，并将设置保存在浏览器的 local storage 中。
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从列表中选择 **Accessibility agent**。

    ![Copilot 云端智能体分配界面，其中突出显示了自定义智能体和 Accessibility](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结与后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是专门针对特定任务和领域设计的 AI 助手。自定义智能体可以将团队专业知识和标准整理为可复用的形式，从而更有效地引导 Copilot 完成特定工作。

本练习中，你探索了：

- 如何定义自定义智能体。
- 如何为自定义智能体分配任务。

Copilot 正在实现高对比度模式；接下来，你将转到[控制中心监控并指导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为在组织中使用自定义智能体做准备][org-custom-agents]
- [为在企业中使用自定义智能体做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
