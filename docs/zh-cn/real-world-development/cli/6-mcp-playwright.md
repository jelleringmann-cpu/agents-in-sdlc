---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "检查或配置 Copilot CLI 中的 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面提到的，编写软件不只是编写代码。我们还要处理数据和外部服务，并允许 Copilot 使用更多自动化功能。MCP 服务器可以让 Copilot 超越 CLI 的内置功能，使用更多工具和服务。

本课将介绍如何：

- 了解模型上下文协议 (MCP) 及 Copilot CLI 如何使用它。
- 如果 Playwright MCP 服务器尚不可用，则添加该服务器。
- 要求智能体操作浏览器并探索筛选功能。

## 场景

单元测试和端到端测试固然重要，但要验证 UI 更新，就需要实际与 UI 交互。你希望 Copilot 能像用户一样使用正在开发的网站，进一步自动化更改流程，并增强对更新符合预期的信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post]为 AI 智能体提供与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时访问最新信息，并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都代表 AI 智能体可访问的一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 托管了 [MCP registry][mcp-registry]，帮助用户发现并参与生态系统。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请审查其源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Playwright MCP 服务器尚不可用，请添加它，让 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，直接继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 在名称字段中输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd>确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 将以下内容粘贴到 **Command** 对话框中：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux），保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd>退出 MCP 对话框。

## 要求 Copilot 通过 Playwright 探索功能

之前你已手动确认功能符合预期。现在使用 Copilot 中的 Playwright MCP 服务器执行同样的检查。

1. 使用以下提示词，要求 Copilot 通过 Playwright MCP 验证功能：

   ```plaintext
   启动应用，并使用 Playwright MCP 对照议题和计划检查筛选功能。不要进行更改，只告诉我哪些功能正常、哪些不正常。完成后停止你启动的服务器。
   ```

> [!NOTE]
> 通常不必告诉 Copilot 使用 MCP 服务器，因为它往往能自行选择。不过，既然你知道它应该使用哪个服务器，明确指出也很有帮助，可以让结果更一致并节省一些 token。

2. 观察 Copilot 在浏览器中执行的步骤。
3. 阅读报告，确认各项行为是否符合预期。

Copilot 会启动服务器，使用 Playwright 与网站交互，完成后停止服务器并提供报告。

## 总结与后续步骤

恭喜！你使用 Playwright MCP 服务器，让 Copilot CLI 在真实浏览器中探索了功能。本课回顾：

- 了解模型上下文协议 (MCP) 及 Copilot CLI 如何使用它。
- 如果 Playwright MCP 服务器尚不可用，则添加该服务器。
- 要求智能体操作浏览器并探索筛选功能。

接下来，你将[创建 QA 自定义智能体][next-lesson]，将技能与浏览器工具结合起来。

## 资源

- [MCP 是什么？为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
