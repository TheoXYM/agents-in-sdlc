---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、ユーザー体験の改善や機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、進捗の監視も重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理する方法はいくつかあります。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。作業内容を調べ、進行中のセッションに追加の指示を出すこともできます。

このレッスンでは、次のことを行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイコントラスト モードの作成を依頼した後、チームはライト モードも追加するとよいと考えました。サイトのスタイル更新と切り替え機能をすでに実装しているため、同じ作業に含めるのが合理的です。ハイコントラストに加えてライト モードも追加するよう、エージェントに指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン パネルと左側のパネルにあるタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲーム変更用の API を生成する。
   - Web サイトにハイコントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。リポジトリのチェックアウト、作業環境の作成、MCP サーバーの設定、タスク完了に向けた各種手順などが含まれます。

> [!NOTE]
> 表示される手順は、作業の状態や Copilot が選んだ方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) パネルも確認します。PR と変更されたファイルを確認できます。

## Cloud agent に追加の指示を出す

実行中のタスクを確認したので、ハイコントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラスト モードの追加に関するセッションを選択します。タイトルは Copilot が付けた名前や作業状態によって異なります。

    ![Mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Agents ページで、作業中の cloud agent セッションに追加指示を出すダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に追加したことを確認します。

## Copilot に作業を続けさせる

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中だった手順を終えた後、新しい要望を作業に取り込みます。

今回も数分かかるため、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページは、GitHub Copilot cloud agent のタスクを監視して指示するための中心的な場所です。実行中や完了済みのタスク、実施中の作業を確認し、進行中のタスクの範囲を調整したり追加の指針を与えたりできます。

このレッスンでは、mission control とエージェント ページを使ってタスクを監視し、進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は Copilot が作成したプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
