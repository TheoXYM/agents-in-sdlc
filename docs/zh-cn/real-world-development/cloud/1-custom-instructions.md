---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就要确保这些上下文随时可用。[指令文件][instruction-files]可提供这类指导，让 Copilot 不仅了解你*要做什么*，也知道*应该如何做*。

在本练习中，你将：

- 了解项目上下文、编码准则和文档标准如何通过存储库自定义指令及按路径限定范围的指令文件传递给 Copilot；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，这里不会运行前后对比提示。Copilot 云智能体会异步处理 GitHub 议题，因此很难实时并排展示指令的影响。你将在本学习路径后续审查云智能体创建的拉取请求时，看到指令文件发挥的作用。

## 指令文件

### 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式，并具有现代感。
- 应以 TSDoc 文档注释的形式为代码添加文档。
- 每个文件顶部都应添加一段注释，说明该文件的作用。

使用指令文件可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和需求。这项功能可以引导 Copilot 给出更相关的建议和代码片段。你可以指定偏好的编码约定、库，甚至希望添加的注释类型。既可以为整个存储库创建指令，也可以针对特定文件类型创建指令，提供任务级别的上下文。

指令文件有两种：

- `.github/copilot-instructions.md` 是一个存储库级指令文件，会随该存储库的**每个**请求发送给 Copilot。文件中应包含项目级信息，即大多数 Copilot 聊天或 CLI 请求都适用的上下文，例如技术栈、项目概况、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型创建。你可以为某种语言（如 TypeScript 或 Astro）提供指导，也可以针对创建 UI 组件或一组单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令以及带有 `applyTo` front matter 的 `*.instructions.md` 文件。
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
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，以确定指令适用的文件。这里，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会符合条件。
5. 留意其中专门针对本项目单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成更小、可复用的文件，并提供可供 Copilot 参考的示例。（这些路径是相对于该指令文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但目前尚未要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加存储库级标准

接下来只需编辑一个文件：添加一条项目级规则，要求使用 TSDoc 文档注释和文件级注释头。后续练习中，云智能体处理你分配的议题时会遵循这条规则。

开始编辑前，先创建一个工作分支（云智能体会读取议题目标分支中的指令，但你仍应整洁地提交自己的更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。我们来补充存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意，该部分已记录项目编码标准，但尚未包含代码文档规则，因此之前生成的 helper 没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件既可以放在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务提供指导。像文档注释规则这样的项目级标准适合放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。在下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建新分支。因此，必须先将这项指令更新合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，然后合并。下一练习中，云智能体处理议题时便会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main` 分支，可以跳过创建分支的步骤，并直接提交到 `main`。这里保留分支步骤，是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何从项目指令文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。后续练习审查云智能体生成的拉取请求时，可以看到这条标准的实际效果。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot——指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
