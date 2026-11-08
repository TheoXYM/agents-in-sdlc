---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用程序更新，通常也需要修改多个文件并执行测试等操作。开发人员往往需要找到所有相关文件、进行修改、运行测试、调试、找出遗漏文件并再次修改，流程可能不断重复。

Copilot 代理模式正适用于此类工作。

Copilot 代理模式可以在 IDE 中自主工作。它会像开发人员一样探索现有项目结构、进行必要更新、运行测试等任务，并自动修复发现的问题。现在来了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云智能体的名称相似，但适用于不同的体验。代理模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解如何：

- 使用 Copilot 代理模式探索项目、查找相关文件并协调多文件更改；
- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能；
- 在将代码合并到代码库之前，审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增加，你希望允许用户按类别和发行商筛选。上一练习已添加 publishers helper，现在将使用 Copilot 代理模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改之前，先浏览 Tailspin Toys 网站，了解现有功能。

该网站是一个以开发者为主题的桌游众筹平台，可列出游戏并显示详细信息。它是一个单独的 Astro 应用程序，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 front matter 中直接查询本地 SQLite 数据库；没有独立的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能则使用标准 DOM API 编写的小型 Astro `<script>`。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。可以在 GitHub Codespace 中按以下步骤运行：

1. 返回 codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器启动后，应会显示类似以下 URL 的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向当前托管网站的 codespace 的私有隧道。

### 浏览网站

网站运行后，可以探索其功能。主要功能包括：

- **主页**：显示桌游列表、标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、描述、发行商和类别等信息。

## 与 Copilot 一起浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新对话。模式和模型选择因练习而异；各步骤会在相关位置说明。

网站的初始实现已可运行，但我们希望增加功能。先查看待办事项。通过模板设置项目时，系统已自动创建一组议题；请 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用了 Claude Sonnet 4.5，因此示例指定了该模型，希望获得相对一致的体验。不过，大型语言模型具有概率性，你看到的建议可能与示例不同，这是正常且预期的情况。

> [!NOTE]
> 由于大型语言模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 发送以下提示，请 Copilot 查看待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 审查生成的议题列表。

请注意，Copilot 还按它认为对用户最有用的顺序排列了议题。

## 审查指令文件

让智能体生成代码之前，先查看将用于提供上下文的指令文件。本练习会使用 [UI 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中包含向网站添加功能时的指导。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于添加功能的整体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍原则；
   - 指向各类文件专用指令的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用指令、拆分复杂内容，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用程序进行三项更新：

- 在数据层 (`src/lib/`) 添加或完善剩余筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页面，加入筛选 UI。

此外，合并前还需要运行并通过测试。Copilot 代理模式可以完成这些任务。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中了 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认当前模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，请 Copilot 根据待办事项中相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找相关文件。它应该会找到数据层 helper、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中代码编辑器可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会出现 **Keep** 或 **Undo** 按钮，无需在工作进行时提前选择。

7. 按 Copilot 的提示选择 **Continue** 运行测试。

   ![Copilot Chat 面板中询问用户是否允许运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能出现暂停，甚至有测试暂时失败。这没有关系！Copilot 会在生成代码和运行测试之间迭代，直至任务完成且未发现错误。

   ![完整 Copilot 代理模式 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 请始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行网站的浏览器，探索新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加功能。你了解了如何：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能；
- 使用 Copilot 代理模式探索项目、查找相关文件并协调多文件更改；
- 在将代码合并到代码库之前，审查代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 加分探索练习——实现分页

随着游戏列表增加，网站需要分页。运用本练习中学到的技能，请 Copilot 更新网站以实现分页。考虑以下方面：

- 遵循现有最佳实践，包括使用已有的指令文件；
- 决定分页方式，例如是否允许用户选择每页大小，还是使用固定值；
- 编写提示时，提供足够指导，让 Copilot 按预期实现功能；
- 可能需要通过后续提示和上下文与 GitHub Copilot 迭代。这是使用 Copilot 的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：用途及使用时机][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
