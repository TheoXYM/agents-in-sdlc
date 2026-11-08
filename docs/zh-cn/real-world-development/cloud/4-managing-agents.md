---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云智能体完成了三个任务，以改善用户体验并添加功能。云智能体可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心中，可以查看所有创建了未关闭拉取请求的智能体任务，了解已执行的操作，还可以引导正在进行的会话。

在本练习中，你将：

- 浏览智能体页面，监控云智能体任务；
- 引导正在进行的会话，要求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以顺便添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此增加浅色模式很合理。现在需要引导智能体，同时实现浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
   - 更新代码文档；
   - 为游戏添加 API；
   - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的步骤，例如：
   - 从存储库检出代码；
   - 创建 Copilot 的工作环境；
   - 设置 MCP 服务器；
   - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 当前的工作状态和执行方案而变化。

4. 还要查看右侧显示的拉取请求面板，可在此查看 PR 和更改的文件，以便进一步监控。

## 引导云智能体

现在已经了解当前正在执行的任务。接下来，在智能体处理高对比度模式的同时，要求它添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 采用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。出现与以下示例类似的消息时，就表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，其中突出显示了引导正在运行会话的对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。它会在完成发送提示时正在执行的步骤后，将新请求加入工作流程。

这同样需要几分钟。你可以稍作停顿，回顾目前学到和探索的内容。

## 总结和后续步骤

本练习介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心。你可以通过任务控制中心跟踪所有正在进行和已完成的任务、查看执行情况，还可以重定向正在运行的任务，以调整范围或提供额外指导。

你了解了以下内容：

- 使用任务控制中心和智能体页面监控云智能体任务；
- 重定向正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
