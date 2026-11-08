---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在 Copilot 代理模式下工作时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些操作会在你的计算机上实时执行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以直接在聊天中查看差异、接受或拒绝单项更改，并通过后续提示引导对话、完善或扩展工作，全程无需离开编辑器。

在本练习中，你将：

- 实时监控无障碍智能体的工作；
- 审查并接受建议的文件编辑；
- 在会话进行期间引导智能体，除了高对比度模式，还添加浅色模式切换开关。

## 场景

[练习 4][previous-lesson]中的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队提出了额外需求：为觉得默认主题太暗的用户添加浅色模式切换开关。你将继续当前会话、扩展工作，而不必从头开始新对话。

## 监控 Copilot 进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联方式显示差异和 **Keep** / **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 随着智能体工作，滚动聊天内容查看每次工具调用。你会看到文件读取、文件编辑，以及它请求运行的终端命令。
3. 智能体提出文件编辑时，差异会以内联方式显示：
   - 选择 **Keep** 接受更改；
   - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看该会话中智能体修改的完整工作集。

> [!NOTE]
> 如果工具调用需要权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式实现。完成时会看到总结更改内容的消息。

## 引导智能体添加浅色模式

高对比度模式已就绪，现在扩展同一对话，添加浅色模式切换开关。由于仍处于同一个聊天会话，智能体会保留之前工作的上下文，包括修改的组件、使用的样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会从之前的工作继续，探索现有更改并提出浅色模式的新增内容。
3. 与高对比度模式一样，逐项审查新编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体采取了不希望的方向，例如重构切换 UI，可以使用聊天输入框旁的 **Stop** 按钮停止它，然后发送澄清提示。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以使用后续提示要求调整（“简化 CSS——使用 CSS 变量，不要重复选择器”）、纠正方向（“不要添加第三个切换开关，将浅色模式整合到现有设置面板”）或扩展范围（“还要为两个切换开关添加键盘快捷方式”）。

## 审查工作集

提交之前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看所有更改文件。应该会看到 Astro 组件、样式和相关测试的更新。
3. 打开部分已更改文件并查看差异。确认其中体现了自定义智能体采用的无障碍模式，例如 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你已使用 Copilot Chat 代理模式监控并引导正在进行的会话：逐项审查文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留现有上下文。

本课中，你了解了以下内容：

- 实时监控智能体工作；
- 接受或拒绝单项文件编辑；
- 使用后续提示引导正在进行的会话。

在[下一课][next-lesson]中，你将把无障碍工作发布为拉取请求，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
