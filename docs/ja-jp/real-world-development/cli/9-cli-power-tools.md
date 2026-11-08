---
title: "レッスン 9 - GitHub Copilot CLI の便利なコマンド"
description: "コンテキストの管理、モデルの選択、セッション共有、任意のクラウド委任に使う slash command を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には操作用の slash command が多数あります。高度な機能、内部情報、追加の構成オプションを利用できます。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。ここでは、他の便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使える任意のコマンドを確認します。

## シナリオ

CLI の主なワークフローが完了しました。次に、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、slash command でコンテキストを調べたり、手動で圧縮したりできます。

1. Codespace に戻り、Copilot CLI が起動していなければリポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分かれているか確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比べます。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的に圧縮します。自分でタイミングを選ぶ場合は `/compact` を使います。関係のない別のタスクに移るときは、`/clear` または `/new` で新しい会話を開始します。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを確認して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで学んだことを共有すると、AI ツールの活用を改善できます。`/share` は、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンにある `/share` のオプションを確認します。
2. セッションを共有する場合は、用途に合った出力先を選びます。たとえば `/share file` はローカルに Markdown をエクスポートします。
3. エクスポートした内容を送信または公開する前に確認します。エクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容を公開しないでください。

## オプション: 作業の拡張や委任

ワークショップの主な作業は完了しました。Copilot CLI には、大規模なタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分配して並列実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、非同期で作業し、プルリクエストを作成する場合もある Copilot cloud agent にタスクを送信できます。

追加の worktree やリモート作業が発生するため、これらのコマンドは任意です。試すときは、範囲の明確な新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントを詳しく学ぶには、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI の slash command を使うと、設定を変更し、セッションを共有し、内部の動作を確認できます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で、Copilot がコンテキストを管理する方法を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使える任意のコマンドを確認しました。

他にも多くの slash command があります。次は、学んだ内容と次のステップを[振り返ります][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
