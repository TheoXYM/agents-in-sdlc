---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "浏览现有的 quality-checks 技能、自定义报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是写代码。我们已经手动验证了代码，并使用指令文件确保代码符合标准。但测试、lint 以及持续集成 (CI) 中的其他流程该怎么办？

这些任务最适合使用**智能体技能**！技能可以帮助 Copilot 了解如何正确执行这些操作。

本课将学习如何：

- 浏览现有的 `quality-checks` 技能；
- 自定义技能的结果报告格式；
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，使结果更易于阅读。

## 指令、脚本和资源

智能体技能会打包可复用的任务指令、可执行脚本和辅助资源，供智能体按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含 Markdown 文件 `SKILL.md`。该 Markdown 文件包含定义技能名称和说明的 front matter、技能用途概览，以及何时调用技能的指导。文件夹还可以包含脚本和其他辅助资源。

> [!NOTE]
> 技能不一定需要其他文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目已有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，成为供团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 浏览技能

现在浏览 Tailspin Toys 团队用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读文件顶部的 `name` 和 `description`。说明有助于 Copilot 判断何时调用该技能。
3. 阅读指令，了解其如何引导 Copilot 执行测试和 lint。
4. 留意目前还没有 **Results output formatting** 部分。

## 修改前先运行技能

可以直接通过 Copilot CLI 调用技能，也可以使用自然语言请求。现在请 Copilot 运行该技能中的三项检查。

1. 返回 Interactive 模式下的筛选功能对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告。

## 自定义报告

现在改进报告，让它说明运行了什么、是否成功，以及工具报告的内容。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更改已完成，现在看看实际效果。Copilot CLI 无需重启对话即可重新加载编辑后的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 再次使用之前的同一提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告，并与首次报告比较。

## 总结和后续步骤

你已自定义并使用现有智能体技能。本课中，你：

- 浏览了用于单元测试、lint 和类型检查的 `quality-checks` 技能；
- 自定义了结果报告格式；
- 重新加载并运行了技能。

这项更改会与筛选功能一起纳入功能 PR。接下来，[通过 Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是额外的练习任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和捆绑的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
