---
slug: ja-jp/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[VS Code の GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** を使うと、普段使用しているコード エディターで GitHub Copilot を利用できます。Visual Studio Code と GitHub Codespaces で Copilot Chat のエージェント モードを操作し、MCP を通じて外部ツールに接続し、カスタム エージェントを活用します。IDE を離れずに作業できるため、Copilot はファイル、ターミナル、問題をすべて把握できます。

まずカスタム指示を追加し、Copilot がその指示に従う様子を確認します。次に、エージェント モードを使って UI、データ層、テストにまたがる絞り込み機能を構築します。続いて Playwright MCP サーバーを接続し、プルリクエストを作成する前に、Copilot にブラウザーを操作させて機能をテストします。最後に、アクセシビリティ作業向けのカスタム エージェントを確認して使い、エディターを離れずに Copilot の変更を監視、調整、反復します。

## 演習

| レッスン | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム指示][ex1] | コンテキスト | VS Code でカスタム指示を追加して検証する |
| [2. エージェント モード][ex2] | コード生成 | エージェント モードで絞り込み機能を構築する |
| [3. Playwright による MCP][ex3] | 外部ツール | Playwright MCP サーバーを使ってブラウザーで機能をテストする |
| [4. カスタム エージェント][ex4] | 専門エージェント | カスタム エージェントを確認して使う |
| [5. エージェントの管理][ex5] | 監視 | エージェント セッションを監視して調整する |
| [6. 反復して改善する][ex6] | レビュー | Copilot の作業をローカルでレビューし、次のステップを選ぶ |
| [オプション: Foundry を組み込む][foundry-toolkit] | AI エージェント | VS Code と Foundry Toolkit を使う 3 つのモジュールで、モデルを準備し、エージェントをデプロイしてサイトに接続する |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランがある GitHub アカウント
- [ ] GitHub Codespaces へのアクセス

> [!TIP]
> 有料プランをお持ちでない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

[callout-student-plan-education]: https://github.com/education/students
## はじめる

**[レッスン 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
