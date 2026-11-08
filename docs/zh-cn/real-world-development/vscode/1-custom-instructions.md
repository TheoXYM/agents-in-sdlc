---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就要确保这些上下文随时可用。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[指令文件][instruction-files]，指导 Copilot 如何生成代码。

在本练习中，你将：

- 了解项目上下文、编码准则和文档标准如何通过存储库自定义指令及按路径限定范围的指令文件传递给 Copilot；
- 在当前指令的基础上，生成筛选功能的第一块数据切片（publishers helper）；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准；
- 重新运行相同提示，观察生成的代码如何采用新标准；
- 将指令更新和筛选功能切片提交到 `main`，供下一练习中的 Copilot 使用更新后的指导。

> [!CAUTION]
> 生成的代码可能与部分标准不完全一致。Copilot 具有非确定性。本练习的目的是观察更新指令后行为变化的*趋势*，而不是要求输出逐字符相同。

## 指令文件

### 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式，并具有现代感。
- 应以 TSDoc 文档注释的形式为代码添加文档。
- 每个文件顶部都应添加一段注释，说明该文件的作用。

使用指令文件可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和需求。可以用它引导 Copilot 给出更相关的建议和代码片段，也可以指定偏好的编码约定、库以及注释风格。你可以为整个存储库或特定文件类型创建指令，以提供任务级别的上下文。

指令文件有两种：

- `.github/copilot-instructions.md` 是一个存储库级指令文件，会随该存储库的**每个**请求发送给 Copilot。文件中应包含项目级信息，例如所用技术栈、项目概况、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型创建。你可以为特定语言（如 TypeScript 或 Astro）提供指导，也可以针对创建 UI 组件或一组单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令以及带 `applyTo` front matter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持采用其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会的范围，不过示例项目展示了一种具有代表性的做法。概括来说：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如项目概况、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用清晰的自然语言说明，并举例展示代码应该和不应该采用的形式。

创建指令文件没有唯一正确的方法，使用 AI 也是如此。通过实验，你会找到最适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的指令文件。浏览本项目中的文件时，你可能会发现，其中包含 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等多种任务的指令。
>
> Copilot 也可以帮你生成指令文件。不同产品界面的入口有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前学习路径会在相关课程中介绍这些功能。
>
> 想寻找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中收集了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花些时间阅读此存储库提供的指令文件，其中包含一个核心 `copilot-instructions.md` 文件，以及一组面向不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容，留意针对 Astro 文件、Drizzle 数据层、测试等的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，以确定指令适用的文件。例如匹配 `**/*.test.ts` 的 TypeScript 测试文件会符合条件。
5. 留意其中专门针对本项目单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并提供可供 Copilot 参考的示例。（这些路径是相对于该指令文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但目前尚未要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

遵循最佳实践，为更改创建分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新对话。模式和模型选择因练习而异；各步骤会在相关位置说明。

为了观察自定义指令的影响，先在当前指令文件生效时发送提示。稍后更新指令，再发送相同提示进行比较。

1. 关闭之前练习留下的编辑器选项卡，让 Copilot 只获取本次需要的上下文。
2. 打开 `src/lib/publishers.ts`，告诉 Copilot helper 应放置的位置。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，这样 Copilot 才能应用文件更改。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改 helper 文件和测试。
6. 留意建议的 helper 是类型化函数，第一个参数接收 `db` 客户端，返回类型化的 publishers 数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 留意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有非确定性，因此即使未明确要求，也可能添加文档注释。如果出现这种情况也没关系；这里要观察的重点仍是更新指令后行为的一致性变化。

## 添加存储库级标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库编码标准补充要求，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意该部分已经记录项目编码标准，但尚未包括代码文档规则，因此之前生成的 helper 没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件既可以放在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务提供指导。像文档注释规则这样的项目级标准适合放在存储库级文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的 Publisher 数据访问辅助函数。
    * 提供从数据库检索 Publisher 信息的函数。
    */
   ```

5. 留意建议的函数现在会包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * 返回所有 Publisher 的列表，包含其 id 和 name。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为 Publisher 对象数组。
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用到真实代码中。下一练习将继续在此基础上构建。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，也需要使用版本控制管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 运行以下命令，确认指令和 helper 更改均存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新以及 Copilot 为 helper 基础所更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求，并关联到待办事项中的文档标准议题。Copilot 可以使用已连接到项目的 GitHub 工具查找议题并代为创建 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已配置好 GitHub 工具，无需另行设置。首次使用工具时，VS Code 可能会提示你登录 GitHub。请按照提示完成操作。后续练习会介绍这种连接的工作原理（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中已选中 **Agent**。
3. 请 Copilot 查找与更新指令文件有关的议题，并从当前分支创建 PR，说明指令更新和 publishers helper 基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 如果系统提示 **Allow** Copilot 代表你执行 GitHub 操作，请先审查命令，再酌情选择 **Allow**。
6. PR 创建后，使用以下提示请 Copilot 合并 PR 并将本地分支切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果系统提示 **Allow** Copilot 代表你执行 GitHub 操作和 shell 命令，请先审查命令，再酌情选择 **Allow**。

现在已经在 GitHub Copilot 的帮助下创建并合并了拉取请求。

## 总结和后续步骤

你已了解 Copilot 如何从项目指令文件中获取上下文，并在 VS Code 的 Copilot Chat 中完成以下工作：

- 发送代码生成提示，观察现有指令下 Copilot 的输出；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 重新发送相同提示，观察建议的代码如何采用新标准；
- 将指令更新和 helper 基础提交到 `main`，供下一练习使用。

接下来，在[代理模式][next-lesson]中应用这些指令，让 Copilot 在整个代码库中添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot——指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
