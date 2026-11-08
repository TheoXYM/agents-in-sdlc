---
title: "第 2 课 - 添加星级评分：快速上手"
description: "使用 Copilot CLI 修改游戏卡片，在转发浏览器中审查，并通过第一个拉取请求合并更改。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试对话后，就可以开始对项目进行第一次更改了。先从一个小任务入手：游戏数据中已有星级评分，但首页的游戏卡片还没有显示它。请智能体显示评分，审查更改，然后通过第一个拉取请求合并它。

本课将学习如何：

- 在功能分支中开始针对特定任务的 Copilot 对话；
- 请智能体对项目进行小幅更改；
- 使用 `/diff` 审查更改；
- 启动应用程序并在 Codespaces 转发的浏览器中确认更改；
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，游戏详情页已经显示评分。但首页的游戏卡片只显示标题、类别、发行商和描述。作为热身练习，请智能体在每张卡片上显示现有评分。这是一个小而独立的更改，很适合作为第一次会话的任务。

## 对话的组成

**对话**是你使用 Copilot CLI 完成任务的工作区。与 Copilot app 不同，普通 CLI 对话会使用终端当前检出的存储库和 Git 分支，而不会创建专用工作树。保存的对话允许你稍后返回同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

对话中包含三类内容：你发送的提示和智能体答复、智能体探索和编辑文件时执行的工具操作，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

现在开始一个新对话，实现这项功能。

1. 返回 Codespace。
2. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，执行以下命令启动：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确认开始新会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，查找显示游戏详情的文件，并编写所需代码。你已使用 Copilot CLI 添加了新功能。

## 审查差异

合并前应审查所有 AI 生成的更改，即使是小改动也不例外。现在在 Copilot CLI 中查看更改。

1. 输入 `/diff`，检查每个更改的文件。
2. 确认游戏卡片在有评分时显示数字评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 查看 Copilot 运行的检查结果，并要求它修复失败项。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性而非确定性，因此生成的代码可能不同。审查行为是否符合预期，而不要要求输出必须逐字一致。

## 检查更改

不要只看代码就假定一切正常。请 Copilot 启动网站，然后在 Codespaces 转发端口打开的浏览器中检查更新后的界面。

1. 请 Copilot 启动应用程序：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了满分为五分的评分。
4. 返回 Copilot，请它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

功能已完成！接下来创建拉取请求 (PR)，将新代码合并到项目中。

1. 请默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，打开 PR。
4. 审查更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main` 分支：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜你，已使用 GitHub Copilot CLI 发布了第一个更改！具体来说，你：

- 在功能分支中开始了针对特定任务的 Copilot 对话；
- 指示智能体修改游戏卡片；
- 使用 `/diff` 审查更改；
- 启动应用程序，并在转发的浏览器中确认评分；
- 创建并合并了第一个拉取请求。

接下来，从[筛选议题开始，使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
