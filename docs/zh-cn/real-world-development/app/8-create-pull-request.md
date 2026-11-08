---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一个分支中。现在一起审查这些更改并创建拉取请求。你已自行合并星级评分拉取请求 (PR)；这次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为简化研讨会，我们将完整的筛选和质量工作流保留在同一个会话和分支中，并通过一个 PR 提交所有更改。

本课将学习如何：

- 了解 Agent Merge 及其如何自动管理合并流程；
- 检查完整的功能 PR 和验证证据；
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证了一项功能。Tailspin Toys 现在希望自动化剩余的 PR 工作，同时让开发人员保留合并授权权。

## 介绍 Agent Merge

**Agent Merge** 会在 GitHub Copilot app 中自动处理合并拉取请求所需的剩余工作。启用后，应用会话会读取 PR 并处理阻碍项——修复失败的持续集成 (CI) 检查、响应审查评论，并在必要时变基——在 GitHub 允许后立即合并。它会在后台运行，应用重启后仍会继续，并在 PR 合并后自动关闭。

到目前为止，你都是自行选择 **Merge pull request**。Agent Merge 可以代为完成此操作，但它编辑代码和合并仍需要明确授权。授予合并权限前，请先审查其允许执行的操作和工作内容。

## 使用 Agent Merge 管理 PR

所有代码创建并审查完成后，让 Agent Merge 管理 PR 流程。

1. 在智能体选择器中选择 **Default agent**。
2. 选择 **Create PR** 旁的下拉菜单。
3. 选择 **Agent Merge**。按钮会显示为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮，启动流程。只从下拉菜单中选择此选项并不会启动流程。

Agent Merge 流程会启动，并将：

- 创建包含标题和说明的拉取请求；
- 如果会话从议题启动，则在说明正文中引用相关议题；
- 对目标分支执行变基或处理潜在合并冲突；
- 监控 CI 流程，确保所有检查通过；
- 监控其他开发人员或 Copilot 代码审查的 PR 反馈，并进行更新以解决意见；
- 可选地在所有检查成功后自动合并 PR。

接下来，让 Agent Merge 在所有检查通过后合并 PR。

5. 授予合并权限前，审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁有勾选标记。
8. 明确要求智能体合并：

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 跟进进度并处理报告的阻塞项。继续前确认 PR 状态为 **Merged**；仅启用了合并权限或智能体仍在运行，并不能证明合并已完成。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护或缺失的权限。继续前应解决这些阻塞项。

## 总结和后续步骤

你已自动执行开发流程中的多个部分，包括生成代码、测试和验证代码，以及管理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动管理合并流程；
- 检查了完整的功能 PR 和验证证据；
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

接下来，[使用现有画布并创建分类画布][next-lesson]，探索使用智能体检查、规划和可视化工作的更多方式。

## 资源

- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
