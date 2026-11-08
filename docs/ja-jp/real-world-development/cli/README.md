---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエストの自動化を学びながら、Tailspin Toys に 2 つの変更を加え、検証して公開します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルで GitHub Copilot をエージェント型のコーディング アシスタントとして利用できます。コードベースの探索、コードの生成、コマンドの実行、外部ツールへの接続をすべてコマンド ラインから行えるため、グラフィカル エディターに切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys の一連の作業を通して学びます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、プロジェクトを確認します。
2. 星評価の変更を加えてブラウザーで確認し、最初のプルリクエスト (PR) を手動でマージします。
3. 絞り込みの Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューします。
4. リポジトリの指示を更新し、絞り込み作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーで絞り込みを確認します。
7. quality assurance (QA) カスタム エージェントを作成し、要件、テスト範囲、検証の証拠をレビューします。
8. 絞り込みの変更全体をレビューし、Agent Merge を使って絞り込み PR をマージします。
9. コンテキスト、モデル、共有、任意のクラウド委任に役立つ slash command を確認します。

このワークショップでは、星評価の変更と、指示の更新、スキルの更新、QA プロファイル、テストを含む絞り込み機能の 2 つの PR を作成します。絞り込みと品質確認の作業では、1 つの会話とブランチを共有するため、各ツールを試しながら作業を積み重ねられます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、基本操作を確認する |
| [2. 星評価を追加する: まずは簡単な変更][ex2] | 最初の変更 | 既存の評価と null の場合の代替表示を行い、最初の PR をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | Issue に基づいて機能を計画し、Autopilot で構築してから Interactive モードでレビューする |
| [4. カスタム指示で Copilot を導く][ex4] | コンテキスト | 指示を確認、更新し、絞り込み機能に適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存のスキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーによる確認 | CLI で MCP を設定し、絞り込みの動作を確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門プロファイルを作成して選択し、最終検証の証拠を集める |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューして PR を作成し、Agent Merge を使う |
| [9. GitHub Copilot CLI の便利なコマンド][ex9] | CLI の機能 | コンテキスト、モデル、共有、クラウド エージェントへの任意の委任を確認する |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、追加リソースを確認する |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログ情報に基づくエージェントをデプロイして Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンド ライン操作の基本的な知識

> [!TIP]
> 有料プランをお持ちでない場合、認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI の使用を有効にしていることを確認してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[foundry]: 8-foundry-agent/
[ex10]: 10-review/
