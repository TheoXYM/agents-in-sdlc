---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "絞り込み、指示、スキル更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込み機能、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて 1 つのブランチにあります。まとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は手動でマージしましたが、今回は **Agent Merge** に任せます。

> [!NOTE]
> 通常は機能、指示、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、絞り込みと品質に関する作業を 1 つの会話とブランチにまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルの自動化方法を学びます。
- 機能の変更全体と検証の証拠を確認します。
- 絞り込み PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

絞り込み機能の作業を通じて、Copilot で計画、実装、検証を行いました。Tailspin Toys では残りの PR 作業を自動化しつつ、マージを許可するかどうかは開発者が判断します。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗やレビュー コメントなど、PR を妨げている問題に対処し、必要に応じてリベースしてから、リポジトリで許可されていれば GitHub の自動マージを有効にします。

ここまでは **Merge pull request** を自分で選択してきました。Agent Merge に任せることもできますが、有効にする前に作業をレビューし、マージしてよいか判断してください。

## Agent Merge で PR を管理する

コードが揃ったので、まとめてレビューし、PR を作成して、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の CI を確認します。すべて成功すると、マージが実行されます。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、必要な権限を回避しません。ブロックされた場合は理由を確認し、リポジトリで許可されている通常の手順でレビュー済みのマージを手動で完了します。

## まとめと次のステップ

コードの生成やテスト、検証に加えて、プルリクエストのプロセスも自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージ ライフサイクルの自動化方法を学びました。
- 機能の変更全体と検証の証拠を確認しました。
- 絞り込み PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR のマージを確認しました。

次は、コンテキスト、モデル、共有、任意のクラウド委任に使える[便利な Copilot CLI slash command][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
