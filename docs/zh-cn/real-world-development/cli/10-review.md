---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的连续开发工作流中使用了 GitHub Copilot CLI。本课中，你：

- 准备了 Codespace、安装 Copilot CLI、探索项目，并找到了预先创建的筛选议题。
- 添加了星级评分，在转发的浏览器中检查结果，并亲自合并了第一个拉取请求 (PR)。
- 从筛选议题出发，在 Plan 模式下确定方案，在 Autopilot 模式下实现，并在 Interactive 模式下审查。
- 使用自定义指令引导智能体，自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议 (MCP) 服务器，并使用它在真实浏览器中探索筛选功能。
- 创建并选择了质量保证 (QA) 自定义智能体，以评估需求、覆盖情况、技能结果和浏览器证据。
- 审查了完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索了用于上下文、模型、共享以及可选 cloud agent 委派的斜杠命令。

## 交付的内容

本工作坊包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating`，并明确显示尚无评分的状态。
2. **筛选功能和质量工作流：**实现筛选功能，更新并应用指令，自定义 `quality-checks` 报告，创建 QA 配置，并加入相关测试。

从规划筛选功能到创建其 PR，你始终使用同一段对话和同一个分支。为简化本工作坊，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自检查浏览器，以及 Copilot 使用 MCP 进行浏览器探索。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。创建 PR 前，QA 将这些结果与需求和测试覆盖审查结合起来。

新增测试应解决实际缺口；QA 运行后不需要新增测试，也可能是合理结果。授权合并前，请审查代码和证据，并在更改后更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了指令、自定义了技能、创建了 QA 配置并配置了 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。指令设定标准，技能描述可重复执行的任务，自定义智能体定义专门角色，MCP 服务器连接外部工具。审查实际更改和工具结果，而不只是智能体的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 在实现前考虑方案，使用 **Interactive** 参与范围明确的更改，使用 **Autopilot** 处理范围清晰的任务。日常编辑可选择更快的模型，复杂工作则选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建*什么*、*为什么*构建以及*如何*构建，会显著影响输出。

## 更多探索

你已经完成了核心工作流。还可以了解 Copilot CLI 的其他实用功能：

- 使用 `/review` 请求代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获取另一种观点。
- 使用 `/fleet` 并行协调彼此独立的子任务。
- 使用 `/worktree` 隔离单独的任务。
- 使用 `/delegate` 将任务委派给 Copilot cloud agent。

## 后续步骤

持续使用工具是熟练掌握它的最佳方式。可以用它处理生产代码、兴趣项目，或那个想了多年却一直没动手的小应用。与团队分享经验，也向团队学习。一如既往，继续探索文档。

若要从终端扩展 Tailspin Toys，请继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，请探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
