---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期留在待办列表中的功能请求。GitHub Copilot cloud agent 能够自主执行更新代码和添加功能等任务。任务完成后，它会创建草稿 PR，供开发人员审查。这可以将繁琐任务交给智能体处理，加快开发流程，让开发人员专注于更重要的工作。

本练习将介绍如何使用 Copilot cloud agent：

- 自定义代码生成环境。
- 确保操作安全执行。
- 了解明确界定任务范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初为网站提供服务的承包商没有妥善编写文档，事实上，你会发现项目完全缺少文档。作为第一步，团队希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队也准备改善游戏发现体验。他们希望游戏详情页显示相关游戏，也就是同一类别中的其他游戏，让用户可以继续浏览。他们暂时不需要完善的实现，只需要一个可用于用户体验验收测试的版本。目前这项工作被其他优先级更高的议题阻塞。

这两项任务都可能很快被推迟，因此适合分配给 Copilot cloud agent。它可以异步处理任务，开发人员则可以专注于其他工作，稍后再回来审查 Copilot 的成果并确认一切符合预期。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。和分配工作给开发人员一样，你可以通过[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)来启动任务。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。你可以在 Copilot 会话进行中或结束后查看它的工作情况。准备好让你审查建议的解决方案时，它会在拉取请求中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 不会变魔术，思考过程不能省略。即使看似简单的操作，深入分析后也可能很复杂，因此[应谨慎界定交给 Copilot cloud agent 的任务范围][cloud-agent-best-practices]。把它当作 AI 结对编程助手：分阶段工作、学习、尝试并不断调整。引入生成式 AI 不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令及其如何引导 Copilot。如果已经完成自定义指令练习，就见过 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。分配工作给 Copilot cloud agent 前，先只读查看存储库现有的指令文件，以便稍后观察它们的影响。

在 GitHub 网页界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter 如何将这些指令限定在 `**/*.test.ts` 文件。

下一节将 `代码缺少文档` 议题分配给 cloud agent 时，留意生成的拉取请求是否采用了 TSDoc 文档注释、TypeScript 约定和文件头部注释。稍后的练习还会再次检查这些内容。

## 为 Copilot cloud agent 设置开发环境

编写代码通常需要特定环境，并运行一些设置脚本以确保环境状态良好。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师的方式工作。

Cloud agent 使用 [GitHub Actions][github-actions] 作为工作环境。你可以通过 `.github/workflows/copilot-setup-steps.yml` 文件配置[专用设置工作流][setup-workflow]，在它开始工作前运行，以便安装所需的开发工具和依赖项。为确保实验流程顺利，这项配置已经预先设置好，可以让 Copilot 使用单个 Astro 应用所需的 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已完成迁移并填充初始数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的 "Actions" 选项卡中测试这些设置步骤。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在由 Astro + Drizzle/libSQL 构成）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移本地 SQLite 数据库并写入初始数据，供构建和测试使用。
      - name: Set up the database
        run: npm run db:setup
```

它看起来与其他 GitHub 工作流文件类似，但有几个关键点：

- 工作流中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，该作业会在 GitHub Actions 中运行。
- 注意 `workflow_dispatch` 触发器。它允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于测试工作流是否成功运行，而不必等 Copilot 启动它。

## 添加文档

虽然人人都知道文档很重要，但多数项目的信息都已过时，或者完全缺少文档。这类技术债务经常得不到处理，会降低效率，也会让维护代码库或让新开发人员加入团队变得更困难。Copilot 擅长编写文档，因此很适合把这项工作分配给 Copilot cloud agent。它会在后台生成所需文档，你将在后续练习中审查结果。

1. 在新的浏览器选项卡中转到 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 在 **Title** 中输入 `代码缺少文档`。
6. 在 **Description** 中输入：

    ```plaintext
    我们组织要求在有助于理解代码时，为函数和方法添加 TSDoc 文档注释。但近期更新没有遵守此标准。我们需要更新现有代码，在文档注释有助于说明行为的地方补充注释。
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

    ![Copilot 议题分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新创建的拉取请求 (PR)，其标题可能类似于 `[WIP]: 代码缺少文档`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，你应该会看到 Copilot 创建的待办事项列表。

> [!NOTE]
> Copilot 创建的待办事项列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表及其计划完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![查看 Copilot 会话](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 你可能需要刷新窗口才能看到状态指示器更新。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并决定合适的计划，以及编写代码。

这通常需要几分钟。Copilot cloud agent 的主要优势之一是能够异步执行任务，让我们专注于其他工作。现在利用这一点，再分配一个任务给 Copilot cloud agent，然后继续开发应用功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的一个重要优势是可以拆分工作：你专注于一项任务，它则处理另一项。为设计团队添加相关游戏部分可能不会花很长时间，但这段时间仍可用于其他工作。现在将这项任务也分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 在 **Title** 中输入 `显示游戏详情页中的相关游戏`。
6. 在 **Description** 中输入：

    ```markdown
    我们希望在每款游戏的详情页上显示相关游戏，帮助用户发现更多游戏。设计团队希望探索用户体验并进行验收测试。需求如下：

   - 在 `src/lib/` 中添加数据访问辅助函数，返回与指定游戏属于同一类别的其他游戏，并排除该游戏自身
   - 在游戏详情页上显示使用该辅助函数的“相关游戏”部分
   - 处理没有相关游戏的情况
   - 为新辅助函数创建单元测试
   - 创建 PR 前，确保所有测试通过
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论中会出现 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在认真处理第二项请求。Copilot cloud agent 的工作方式类似软件工程师，因此你不需要一直监控它；完成后再审查即可。接下来探索并使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以将议题分配给 cloud agent，让它异步处理技术债务、创建新功能，或协助在不同框架之间迁移代码。

本练习中，你探索了：

- 自定义代码生成环境。
- 确保操作安全执行。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

Cloud agent 在后台继续工作时，我们可以转而创建和使用自定义智能体。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，还能够读取前面模块中介绍的自定义指令。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
