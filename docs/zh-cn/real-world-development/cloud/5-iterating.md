---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你与 GitHub Copilot 协作完成了多个任务，以改善用户体验并添加功能。你让 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换在内的无障碍功能。现在来检查代码更改；必要时向 Copilot 提供反馈，改进其工作。

### 场景

如前面多次强调的，生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能所做的更改，然后再为团队启用代码审查。

## GitHub Copilot 云智能体的安全性

Copilot 云智能体会异步且无人监督地执行任务，因此设有一些安全限制：

- Copilot 对存储库只有读取权限，对用于提交代码的分支只有写入权限。
- 云智能体在 GitHub Actions 中运行，并在独立、临时的环境中工作。
- GitHub Actions 工作流必须经人工批准才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括对 MCP 服务器的访问。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个 PR：为代码添加文档。通过 GitHub.com 上的标准 PR 界面完成此任务。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**正常现象**：Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。你可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 查看拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 或更完整描述的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。此时请等待 Copilot 完成工作。这可能需要几分钟，你可以稍作休息，或回顾目前学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡，审查所做的更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加的代码，包括 TSDoc 文档注释和其他文档。具体更改会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些来自你在练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

9. 工作流应会在拉取请求的检查部分排队运行。如果一切正常，单体 Astro 应用程序的项目检查将通过。此步骤可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作并非单向过程。你可以像 @提及团队成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取评论，并启动新的会话来处理请求。由于结果具有非确定性，我们无法提供固定的提示文本。你可以要求 Copilot：

- 在每个代码文件顶部添加简短的文件注释头；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建一份说明 Astro 应用程序结构的 README。

1. 添加评论，请求 Copilot 更新生成的文档，并像提及用户那样提及 **@copilot**。可以使用上面的建议，也可以提出其他文档改进要求。
2. 选择 **View Session**，观察 Copilot 开始新会话并执行更新。
3. 选择 **Back to pull request** 返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

你可以继续迭代，直到满意为止。完成后，可将 PR 从草稿转换为可审查状态，然后合并到主分支。

![将 PR 转为可供审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来查看 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更新无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流指示器。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

8. 工作流应会在拉取请求的检查部分排队运行。如果一切正常，单体 Astro 应用程序的项目检查将通过。此步骤可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已创建相关游戏功能。与之前一样，你可以继续迭代并要求云智能体更新，例如调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令）。与之前一样，可以在 **Conversation** 选项卡中发表评论。Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查由无障碍自定义智能体实现的功能。这个 PR 应包含你在练习 3 中分配的高对比度模式，以及你在练习 4 的任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。此时请等待 Copilot 完成工作。这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其留意：
   - 用于切换模式的 UI 组件；
   - 用于保存用户偏好的本地存储；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 确认更新无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows 按钮](../../../_images/shared-approve-workflows.png)

9. 工作流应会在拉取请求的检查部分排队运行。如果一切正常，单体 Astro 应用程序的项目检查将通过。此步骤可能需要几分钟。
10. **可选：**可以切换到 Codespace 中的此分支，手动测试无障碍功能。打开 Codespace 和终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用程序，在浏览器中测试高对比度和浅色模式切换，并确认页面重新加载后仍会保留设置。

无障碍自定义智能体帮助 Copilot 根据无障碍最佳实践实现了这些功能。如果发现无障碍问题或改进机会，可以像之前那样在评论中提及 **@copilot**，请求更新。

## 可选练习——继续委派工作

云智能体可以帮助你处理真实的积压工作，让你有时间专注于其他任务。可以在自己的存储库中创建更多议题并分配给 Copilot，练习这一工作方式。例如：

- 在游戏详情页创建支持者兴趣表单；
- 为游戏列表页实现分页；
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成云智能体学习路径，并完成了以下工作：

- **检查了本存储库提供的自定义指令文件**，以便稍后观察它们对云智能体输出的影响；
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务；
- **创建并使用无障碍自定义智能体**，添加高对比度和浅色模式切换；
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导无障碍会话；
- **审查并迭代云智能体创建的拉取请求**，通过提及 `@copilot` 请求更改，并批准工作流。

## 回顾和后续步骤

你已完成云智能体学习路径。还可以通过其他学习路径继续探索：

- 🖥️ **[VS Code 学习路径](../../vscode/)**——在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)**——在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接流程。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体；
- 为其他领域创建自定义智能体；
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
