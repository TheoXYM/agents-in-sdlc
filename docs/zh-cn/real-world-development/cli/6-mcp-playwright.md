---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "检查或配置 Copilot CLI 中的 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是写代码。我们还要处理数据、外部服务，并允许 Copilot 使用其他自动化工具。这正是 MCP 服务器的用途。MCP 服务器可以扩展 Copilot CLI 的内置功能，提供更多工具和服务。

本课将学习如何：

- 了解模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它；
- 如果尚未配置，则添加 Playwright MCP 服务器；
- 请智能体驱动浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但要验证 UI 更新，还需要实际与界面交互。你希望 Copilot 能像用户一样使用正在开发的网站，以自动化方式验证更改是否符合预期。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post]让 AI 智能体能够与外部工具和服务通信。使用 MCP 后，AI 智能体可以实时访问最新信息，并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，每个服务器代表一组可供智能体访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助发现并参与生态系统。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前先检查源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Playwright MCP 服务器尚未启用，现在将它添加到 Copilot，以便 Copilot 能像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，直接继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 将名称设置为 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中输入：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 请 Copilot 使用 Playwright 探索功能

之前你已手动确认功能符合预期。现在使用 Copilot 可用的 Playwright MCP 服务器重复验证。

1. 使用以下提示，请 Copilot 通过 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自行判断应使用 MCP 服务器，无需明确要求。不过，指出要使用的工具有助于获得更一致的结果，也能节省一些 token。

2. 观察 Copilot 如何列出浏览器中的操作步骤，以确认功能是否正常。
3. 阅读报告，确认所有行为符合预期。

Copilot 会启动服务器，使用 Playwright 与网站交互，停止服务器，然后提供报告。

## 总结和后续步骤

恭喜，你已从 Copilot CLI 使用 Playwright MCP 在真实浏览器中探索功能！本课中，你：

- 了解了模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它；
- 如果 Playwright MCP 尚未配置，则添加了服务器；
- 请智能体驱动浏览器并探索筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专用角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
