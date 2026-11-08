---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚才使用代理模式构建了筛选功能。创建拉取请求之前，应确认它在浏览器中实际运行正常。你将连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器并测试功能，而不是自己逐一点击应用程序。随后发布分支并创建 PR。

本练习将学习如何：

- 了解模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具；
- 在工作区中添加 Playwright MCP 服务器；
- 请 Copilot 使用 Playwright 在浏览器中手动测试筛选功能；
- 发布分支并为筛选工作创建拉取请求。

## 什么是模型上下文协议 (MCP)？

代理模式可以借助 MCP 超越编辑器的范围。MCP 是 Copilot 与外部工具和服务通信的标准方式。

![代理模式的工作原理示意图，显示它如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)让 AI 智能体能够与外部工具和服务通信。使用 MCP 后，AI 智能体可以实时访问最新信息（通过资源）并代表你执行操作（通过工具）。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具之间的桥梁，并负责管理通信；外部工具既可以是现有 API，也可以是 npm 包等本地工具。每个 MCP 服务器代表一组智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API，允许 AI 智能体创建和更新存储库、管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，允许 AI 智能体访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器，可提供各种工具和资源。GitHub 提供 [MCP 注册表](https://github.com/mcp)，帮助发现并参与生态系统。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前先检查源代码、验证发布者，并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 文件中应已为你配置 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` 条目已包含在项目模板中，因此 Copilot 能在上一练习中读取待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装——VS Code 通过 HTTP 连接，Copilot 首次使用其工具时需要使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正适合用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，使文件如下所示：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志使 Playwright 在没有可见窗口的情况下运行浏览器，这在没有桌面环境的 codespace 中是必需的。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已经安装了 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器——Copilot 首次需要某服务器工具时，VS Code 会启动该服务器，并询问是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器，确认配置正确。
2. 如果 VS Code 询问是否信任该服务器，请先审查配置，再选择信任以启动服务器。

手动启动是可选的。如果跳过，VS Code 会在下一步中 Copilot 首次需要服务器工具时启动服务器，并询问是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的真实浏览器。智能体可以打开页面、导航、应用筛选条件并读取结果，然后向你总结观察结果。你无需离开对话即可快速验证功能是否符合预期。

Playwright MCP 服务器会读取页面的[无障碍树][playwright-mcp-server]，而不是截图。因此，智能体会根据带标签的结构化元素（按钮、链接、列表项）进行推理，与辅助技术的工作方式相同；快速功能检查也可以顺便验证一些基础无障碍行为。

连接服务器并启动应用程序后，请 Copilot 测试刚刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步操作并汇报结果。对照议题中的验收标准审查结果。如果发现问题，可以提出后续问题，或在创建 PR 前要求 Copilot 修复代码。

> [!NOTE]
> 测试时应用程序必须在 `http://localhost:4321` 运行。如果已停止开发服务器，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面 (CLI) 创建拉取请求。也可以请 GitHub Copilot 查找相关议题并关联创建 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 在智能体下拉列表中确认选中了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 请 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 按需选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时，可能会提示你使用 GitHub 身份验证；按照提示完成操作。
5. 留意 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具；
- 在工作区中添加了 Playwright MCP 服务器；
- 使用服务器在浏览器中手动测试筛选功能；
- 发布了分支并为筛选工作创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等特定任务。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
