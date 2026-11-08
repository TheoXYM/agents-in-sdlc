---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、浏览项目并找到了已创建的筛选议题；
- 添加了星级评分，在转发的浏览器中审查结果，并手动合并第一个拉取请求 (PR)；
- 从筛选议题开始，在 Plan 模式中确定方案、在 Autopilot 模式中构建，并在 Interactive 模式中审查；
- 使用自定义指令引导智能体、自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查；
- 添加 Playwright 模型上下文协议 (MCP) 服务器，并在真实浏览器中探索筛选功能；
- 创建并选择质量保证 (QA) 自定义智能体，以评估需求、覆盖范围、技能结果和浏览器证据；
- 审查完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR；
- 探索用于管理上下文、模型和共享的斜杠命令，以及可选的云委派功能。

## 交付的内容

本研讨会包含两个 PR 里程碑，每个都从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建其 PR，你一直使用同一个对话和分支。为简化研讨会，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、自己在浏览器中检查，以及使用 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 在 PR 创建前综合检查这些结果、需求和测试覆盖范围。

添加的测试应解决真实缺口；QA 运行后无需新增测试也可能是正确结果。合并前请审查代码和证据；发生更改后，重新生成受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作。本研讨会中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。这些自定义项可以在不同对话中复用，并根据团队需求调整。指令定义标准，技能描述可重复的任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不只是智能体的摘要。

根据任务选择**模式和模型**。使用 **Plan** 先考虑方案，使用 **Interactive** 协作完成范围明确的更改，使用 **Autopilot** 处理范围清晰的任务。常规编辑可选择速度较快的模型，复杂任务则可选择能力更强的模型。

上下文与基础设施同样重要。明确描述希望构建的内容、原因和方式，会显著影响输出。

## 继续探索

你已完成核心工作流。还可以探索以下 CLI 功能：

- `/review`：请代码审查智能体分析更改；
- `/rubber-duck`：讨论问题并获得另一种视角；
- `/fleet`：并行协调独立子任务；
- `/worktree`：隔离单独任务；
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

想要熟练使用工具，最好的办法就是持续使用它！可以用它编写生产代码、兴趣项目，或多年一直想构建却还没开始的小应用。与团队分享经验、互相学习，并继续探索文档。

如果想继续在终端中扩展 Tailspin Toys，可以学习可选的 [Foundry Backer Concierge 系列][foundry]。若要对比其他环境，还可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

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
