---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で Issue に基づく絞り込み機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

まず小さな機能を追加しました。より大きな変更には、さらに確かなプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作し、必要なものを適切な方法で構築できるようにします。このレッスンから、Issue に基づく機能の生成、コードの検証、動作確認、プロジェクトへのマージまで、一般的なエージェント駆動の開発プロセスを実践します。

> [!NOTE]
> 機能の作業を続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot の新しい会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新しい機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリや発行元でゲームを絞り込む必要があります。バックログの Issue には機能が記されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細があります。Plan モードで決定し、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、より重要になります。一般的な開発フローは次のようなものです。

1. 必要な作業を記述した Issue から始める。
2. 構築する内容を計画する。
3. コードを構築してレビューする。
4. テストを実行してコードを検証する。
5. 新機能を手動で検証する。
6. プルリクエスト (PR) を作成する。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージする。

> [!NOTE]
> 具体的な内容はチームや組織によって異なりますが、多くは上記の流れを調整したものです。

この標準的な方法を守ることで、AI が生成したコードが要件を満たし、手書きのコードと同じレビュー手順を通るようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> でモードを切り替えます。

- **Interactive**: ユーザーとエージェントが一緒に作業します。エージェントは変更案を提示し、入力を待ってから続行します。
- **Plan**: エージェントが先に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントがコードの作成、テストの実行、反復を自律的に行い、タスクの完了を目指します。

まず Plan モードで計画をレビューしてから、Autopilot で実装します。

## Issue から始める

絞り込みの作業を始める前に、Codespace とターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してから、絞り込み用ブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) から始まることを確認します。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## 絞り込み機能を計画する

計画を立てると、Copilot に任せる前に機能やタスクの実装方法を決められます。複雑な作業では、事前に計画を立てることをお勧めします。Plan モードに切り替えて計画の作成を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 前の手順で追加した Issue 参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. 絞り込みの動作について質問された場合は、希望に応じて回答します。決まった正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画を確認します。
6. 決定した内容に合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画の作成とレビューが終わったら、実装に進みます。Autopilot を使うと、Copilot は完了と判断するまで課題に取り組み、反復します。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みのバージョンに表示される同様の選択肢を選びます。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画を確認してください。依存関係が不足している、またはポートが競合していると報告された場合は、チェック完了と判断する前に環境の問題を解決します。

## 実装をレビューして検証する

他のコードと同様に、生成されたコードはマージ前にレビューします。コードを確認し、サイトを起動して動作も確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、絞り込み機能の実装とテストを確認します。
3. 結果を Issue および計画時の決定と照合します。
4. レビュー後、<kbd>Esc</kbd> を押して diff ビューを終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードを確認できました。実際に動くか確かめましょう。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot に次のプロンプトを送り、アプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリ、発行元、それらの組み合わせで絞り込みます。
4. リセットと該当結果がない場合の動作が、Issue と決定内容に合うことを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

会話モードを切り替えて機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot の新しい会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は[カスタム指示を使って][next-lesson]、生成されたコードが文書化された規約に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
