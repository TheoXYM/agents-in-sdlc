---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都已保存在同一个分支中。现在一起审查这些更改并创建拉取请求。本次将使用 **Agent Merge** 管理合并流程；星级评分 PR 则是由你手动合并的。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为简化本研讨会，我们将筛选和质量工作流保留在同一个对话和分支中，并通过一个 PR 提交所有更改。

本课将学习如何：

- 了解 Agent Merge 及其如何自动管理合并流程；
- 检查完整的功能更改和验证证据；
- 创建筛选功能 PR；
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选工作流中，你使用 Copilot 规划、实现并验证了一项功能。Tailspin Toys 现在希望自动化剩余的 PR 工作，同时让开发人员保留合并授权权。

## 介绍 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的事项——修复失败的持续集成 (CI) 检查、响应审查评论，并在必要时变基——然后在存储库允许时启用 GitHub 自动合并。

到目前为止，你都是自行选择 **Merge pull request**。Agent Merge 可以代为完成此操作，但启用前仍应审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码已完成。现在一起审查、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。一切成功后，它会执行合并。
7. 确认 Copilot 显示了类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的批准、分支保护、合并队列、存储库设置或权限要求。如果流程受阻，请查看报告的原因；在存储库允许的情况下，完成审查后手动合并。

## 总结和后续步骤

你已自动化开发流程中的多个部分，包括生成代码、测试和验证代码，以及管理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动管理合并流程；
- 检查了完整的功能更改和验证证据；
- 创建了筛选功能 PR；
- 审查后启用 Agent Merge，并确认 PR 已合并。

接下来，[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型和共享，并了解可选的云委派功能。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
