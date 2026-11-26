---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[**GitHub Copilot cloud agent**](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)可在云端异步处理工作。你在 GitHub 上分配任务后，cloud agent 会在后台接手：探索存储库、进行更改并创建拉取请求；你则可以继续处理其他事情。

在这些练习中，你将添加 cloud agent 会遵循的自定义指令，然后分配 GitHub 议题，让它实现任务。你还将审查并使用自定义智能体来引导其工作方式，在智能体仪表板中监控和指导会话，最后审查它创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 准备工作][ex0] | 设置 | 创建存储库并启动 codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加 cloud agent 会遵循的自定义指令 |
| [2. Cloud agent][ex2] | 异步智能体 | 将议题分配给 Copilot cloud agent |
| [3. 自定义智能体][ex3] | 专用智能体 | 审查并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并指导智能体会话 |
| [5. 迭代改进][ex5] | 审查 | 审查 PR、迭代改进 Copilot 的工作，并选择后续步骤 |

## 准备工作

参加本工作坊前，请确认已准备好：

- [ ] 拥有有效的 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划的 GitHub 帐户
- [ ] 已为你的帐户或组织启用 Copilot cloud agent

> [!TIP]
> 没有付费计划？通过 [GitHub Education][callout-student-plan-education] 验证的学生可以免费使用 GitHub Copilot。**Copilot Student** 计划包含本工作坊使用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可以完成所有练习。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot cloud agent 适用于 **Copilot Student、Pro、Pro+、Business 和 Enterprise**，仅 Copilot Free 计划除外。对于 Copilot Business 或 Enterprise，管理员必须为组织启用此功能。

> [!NOTE]
> 本练习不涉及 MCP。若要了解如何在 Copilot 中使用 MCP 服务器，请参阅 [CLI 练习][cli]或 [VS Code 练习][vscode]。

## 开始

**[从练习 0：准备工作开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
