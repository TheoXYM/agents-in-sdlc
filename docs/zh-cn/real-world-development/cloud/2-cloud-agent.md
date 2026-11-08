---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。完成后，它会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体处理，加快开发进度，让开发人员专注于更重要的工作。

你将在本练习中了解：

- 如何自定义代码生成环境；
- 如何确保操作安全；
- 如何明确限定议题范围；
- 如何将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初为网站开发的承包商没有留下完善的文档，事实上，代码完全缺少文档。团队希望首先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能。他们想在每款游戏的详情页显示相关游戏，即同类别的其他游戏。暂时不需要完善的实现，只需要能够用于用户体验验收测试的版本。这项工作目前是一个阻碍，但优先级低于其他议题。

这类任务很容易被推迟，因此适合分配给 Copilot 云智能体。它可以异步处理任务，让开发人员继续处理其他工作，稍后再回来审查结果。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。你可以通过多种方式分配工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。你可以在会话进行中或完成后查看详情。建议方案准备好供审查时，Copilot 会在拉取请求中通知你。

## 明确限定任务范围的重要性

GitHub Copilot 并非魔法——它无法替你省去思考。即使看似简单的操作，也可能暗藏复杂性，因此请留意[如何限定分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。把它当作 AI 结对编程伙伴：分阶段工作、学习、试验并逐步调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果你已完成自定义指令练习，就已看到 `.github/copilot-instructions.md` 的效果；如果还没看过，现在可以快速浏览。在分配工作前，以只读方式检查本存储库附带的指令文件，稍后便能观察它们的影响。

在存储库的 GitHub Web UI 中，或已启动的 codespace 中，打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` front matter，它将指令限定在 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` 议题分配给云智能体时，留意生成的 PR 是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些要求来自上述指令文件。后续练习审查 PR 时还会再次提及。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境和设置脚本，确保依赖项和工具就绪。将任务分配给 Copilot 时也一样，它会像软件工程师一样执行任务。

云智能体使用 [GitHub Actions][github-actions] 作为工作环境。你可以在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]，在它开始工作前运行，以安装所需的开发工具和依赖项。为确保实验流程顺利，本项目已预先配置此工作流，为 Copilot 安装 Node.js、项目依赖项、端到端测试所需的浏览器，以及单体 Astro 应用程序使用的已迁移和填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会获得自己的操作令牌。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

这看起来与其他 GitHub 工作流类似，但有几点需要注意：

- 它只包含一个名为 `copilot-setup-steps` 的作业。该作业会在 Copilot 开始处理 PR 之前于 GitHub Actions 中运行。
- `workflow_dispatch` 触发器可让你从存储库的 **Actions** 选项卡手动运行工作流。这有助于测试工作流是否成功，无需等待 Copilot 启动。

## 添加文档

大家都知道文档很重要，但许多项目的文档要么已经过时，要么完全缺失。这类技术债务往往无人处理，会拖慢工作效率，并提高维护代码库或引入新开发人员的难度。Copilot 很擅长生成文档，因此非常适合将这项工作分配给云智能体。它会在后台生成文档，你将在后续练习中回来审查。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的 PR。标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中还没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应该会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中创建待办事项列表。它需要设置环境（运行前面介绍的工作流）、分析项目并确定处理任务的方案。

13. 查看 Copilot 即将完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并制定计划，以及编写代码。

这项工作可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让你腾出时间处理其他工作。接下来会利用这一点：先再分配一个任务给云智能体，然后转而为应用程序编写代码。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你处理一组任务，它处理另一组。即使为设计团队添加相关游戏部分不一定需要太久，这仍然可以节省时间。现在就将此任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，你会在议题的第一条评论中看到一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。云智能体的工作方式类似于软件工程师，因此你不需要一直监控它；等任务完成后再审查即可。现在来了解如何创建和使用自定义智能体。

## 总结和后续步骤

本练习介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理技术债务、新功能，或将代码从一种框架迁移到另一种框架。

你了解了以下内容：

- 自定义代码生成环境；
- 确保操作安全；
- 明确限定议题范围的重要性；
- 将议题分配给 Copilot。

云智能体正在后台处理任务。接下来将创建并使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并可使用前面练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

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
