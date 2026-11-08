---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連の作業を行いました。次のことを実践しました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意された絞り込み Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、`quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザー上の証拠を評価しました。
- 絞り込み機能全体をレビューし、Agent Merge による PR のマージを許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使う slash command を確認しました。

## 完了した作業

このワークショップには、更新済みの `main` から作成する 2 つのブランチと PR があります。

1. **星評価**: ゲームカードに既存の `starRating` と未評価状態を明示して表示します。
2. **絞り込みと品質ワークフロー**: 絞り込みを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連するテストを追加します。

絞り込みの計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの変更を 1 つの PR にまとめています。

## 検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作でコードを確認しました。`quality-checks` スキルでユニットテスト、lint、型チェックを実行し、追加した形式で結果を報告しました。QA は要件とテスト範囲のレビューを、これらの結果と組み合わせました。

追加するテストは実際の不足を補うものである必要があります。QA の結果、新しいテストが不要になる場合もあります。マージを許可する前にコードと証拠をレビューし、変更後は影響を受ける証拠を再確認してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでカスタマイズを再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し行う作業をまとめ、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前の検討、**Interactive** は作業に関与しながら進める変更、**Autopilot** は範囲の明確なタスクに使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

環境と同様に、コンテキストも重要です。何を、なぜ、どのように構築したいかを明確に伝えると、出力が大きく変わります。

## さらに試す

主なワークフローを学びました。Copilot CLI には、他にも便利な機能があります。

- `/review` でコードレビュー エージェントに変更を分析させます。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送ります。

## 次のステップ

どのツールも、使い続けることで上達します。本番コード、趣味のコード、ずっと作りたかった小さなアプリなどで活用してください。チームで学んだことを共有し、互いの経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張するには、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境も体験するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
