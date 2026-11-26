---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流、审查拉取请求，并确认 Space Quiz 已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

将 Space Quiz 部署到 GitHub Pages，为它提供公开访问入口。Copilot 可以准备工作流并引导部署，但你仍需审查更改并确认网站已上线。

本课将介绍如何：

- 要求 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟进部署并确认网站已发布。

## 场景

Space Quiz 已在 GitHub 存储库中，但其他人还不方便直接体验。使用 GitHub Pages 发布这个单文件网站，即可通过公开 URL 访问，无需添加服务器或运行时依赖项。

## 要求 Copilot 准备部署

> [!WARNING]
> 创建拉取请求可能会提交并推送代码。发送此提示词前，请检查批准设置和目标存储库。启用 **Approve all** 后，不要假设系统还会单独提示确认。

在 `space-quiz` 的 Copilot app 会话中发送以下提示词：

```plaintext
为此存储库准备 GitHub Pages 部署。创建 GitHub Actions 工作流：如有需要则构建网站，并在更改进入默认分支后部署到 GitHub Pages。如果存储库中还没有 README，请添加一个，介绍应用以及如何在本地运行。使用这些更改创建拉取请求，并报告链接以及所需的 Pages 设置或权限。不要合并、启用自动合并、更改 Pages 设置或部署。完成后停止并等待我审查。
```

## 审查工作流和拉取请求

1. 确认只有在存储库原本没有 README 时才添加了 README。
2. 审查拉取请求的更改，包括工作流的构建和部署步骤、触发条件及权限。
3. 确认工作流从默认分支部署，而不是从尚未合并的拉取请求部署。
4. 如需修正，请提出要求并审查更新后的差异。授权合并前，等待必需的拉取请求检查全部通过。

## 授权合并和部署

> [!WARNING]
> 以下提示词授权智能体合并已审查的拉取请求、更改存储库的 Pages 设置并发布网站。只有完成审查并确认存储库和拉取请求无误后，才发送此提示词。根据批准设置，这些操作可能不会再次要求确认。

将 `<PR_NUMBER>` 替换为已审查的拉取请求编号，然后单独发送以下提示词：

```plaintext
我已审查此存储库中的拉取请求 <PR_NUMBER>，并授权合并和部署。请使用 GitHub Actions 启用 GitHub Pages；只有必需的检查全部通过后才合并该拉取请求，并持续监控部署直至成功。请报告已上线网站的最终 URL。如果你没有启用 Pages 或合并的权限，请准确说明我需要批准或执行什么操作；网站实际上线前不要报告成功。
```

![三阶段示意图：已合并的 space-quiz 拉取请求；GitHub Actions 的 Deploy website 运行成功，其中包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及位于 yourname.github.io/space-quiz/ 的在线 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

## 验证部署

1. 跟进 GitHub Actions 部署运行，直到运行成功。
2. 打开报告的 URL，确认 Space Quiz 已加载。

如果智能体无法启用 Pages 或合并拉取请求，请按它提供的具体说明操作。部署成功且网站正常加载之前，不要将任务视为已完成。

## 总结与后续步骤

Space Quiz 现已作为在线网站发布；你也亲自确认了部署结果，而不是只依赖智能体的报告。接下来继续学习[第 13 课：回顾与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
