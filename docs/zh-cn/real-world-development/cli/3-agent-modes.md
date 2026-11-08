---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一个小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作流，帮助你以正确的方式构建正确的内容。本课将开始一组课程，介绍典型的智能体驱动开发流程：从议题开始创建功能、验证代码、检查功能是否符合预期，最后成功合并到项目中。

> [!NOTE]
> 后续功能工作将使用相同的对话和分支。实际工作中，通常会为不同文件类型创建不同分支或 PR；本研讨会使用快捷方式，以便专注于核心概念。

本课将学习如何：

- 从 GitHub 议题开始新的 Copilot 对话；
- 在 Plan 模式中明确需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

继续实现此功能时，你还将更新存储库指令、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商缩小游戏范围。待办事项中的议题描述了此功能，但组合类别等细节需要在编写代码前达成共识。你将使用 Plan 模式确定这些细节，再授权 Autopilot 实现范围明确的功能。

## 背景

在开发流程中加入 AI 编码智能体并不会改变基本原则，反而使这些原则更加重要！大多数开发人员遵循的流程大致如下：

1. 从说明工作内容的已提交议题开始。
2. 制定构建方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这些标准步骤，可以确保 AI 生成的代码满足需求，并经历与手写代码相同的审查流程。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先创建计划，期间不能编辑项目文件。
- **Autopilot**：智能体自主工作——编写代码、运行测试并迭代，直至任务完成。

先在 Plan 模式中审查计划，然后使用 Autopilot 实现。

## 从议题开始

开始筛选功能工作前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题加入提示，然后返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 议题或拉取请求 (PR) 加入上下文。

## 规划筛选功能

规划能让你先确定功能实现方案，再交给 Copilot 执行。对于复杂任务，预留时间规划总是值得的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方显示 **Plan**。
2. 在上一步添加的议题引用之后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始探索项目并确定最佳方案。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答，没有错误答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动审查计划。
6. 如果计划中的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划已编写并审查，现在开始实现。让 Copilot 使用 Autopilot 完成工作。

Autopilot 会让 Copilot 持续迭代，直到判断任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前安装版本中含义相同的选项。
2. 确认提示下方显示 **Autopilot**。
3. 观察 Copilot 如何根据计划进行迭代、生成代码并运行测试。

> [!NOTE]
> 批准后会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，和其他代码一样，合并前需要审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 对照议题和规划时做出的决定审查结果。
4. 审查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的结果，并要求 Copilot 解决任何失败项。

## 探索新功能

代码看起来没问题，但能否运行？现在像之前一样启动应用程序，通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用程序：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中约定的组合方式。
4. 确认重置和空结果的行为符合议题和计划中的决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同的对话模式构建和审查功能。本课中，你：

- 从 GitHub 议题开始新的 Copilot 对话；
- 在 Plan 模式中明确需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

接下来，[使用自定义指令][next-lesson]深入了解代码生成，确保生成的代码遵循书面实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
