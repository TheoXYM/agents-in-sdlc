---
title: "レッスン 1 - GitHub Copilot app のインストール"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、GitHub Copilot CLI を基盤にした、エージェント駆動の開発向けデスクトップ アプリケーションです。GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。自分ですべての作業を行うのではなく、複数のエージェントを個別の分離されたワークスペースで並列に指示し、繰り返し作業を自動化するワークフローに適しています。Node.js をインストールし、プロジェクトのコピーを用意できたので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートによって用意されたバックログなど、ワークスペースを確認します。
- クイックチャットを試して、アプリ自体について質問します。

## シナリオ

チームは、増え続けるバックログに取り組むため AI エージェントを導入しています。Copilot app では、Issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise の場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして構成する

まず GitHub Copilot app をインストールします。Windows、macOS、Linux 版が利用できます。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディング ページ][download-app]を開きます。
2. 使用するプラットフォーム向けのアプリをダウンロードし、ページに記載された手順でインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って認証します。データ所在地が有効な GitHub Enterprise Cloud を使う場合は、**Use GitHub Enterprise** を選び、求められたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディングでリポジトリを選択する場合は、テンプレートから作成した Tailspin Toys リポジトリを選びます。リポジトリの選択をスキップし、オンボーディング後に追加することもできます。
6. テーマを選ぶよう求められたら選択し、オンボーディングを完了します。
7. サイドバーの **Sessions** の下にリポジトリが表示されるか確認します。すでに表示されていれば、再度追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選びます。前提条件のレッスンで控えた所有者名とリポジトリ名を使います。
9. リポジトリがサイドバーに表示されることを確認します。このレイアウトでは `tailspin-toys` などのリポジトリ フォルダーが **Sessions** の下にまとまり、その下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、アプリの使い方を確認します。サイドバーには次の領域があります。

- **New** — Copilot との新しいチャット セッションを開始します。
- **Pull requests** — アプリの GitHub 統合でプルリクエストを表示します。参照やフィルタリング、CI 状態の確認、レビューをアプリ内で行えます。
- **Issues** — Issue の参照やフィルタリング、Issue からのセッション開始ができます。
- **Automations** — スケジュールまたはオンデマンドで実行する保存済みエージェント タスクです。To-do リスト、定期保守、繰り返し作業などに使えます。まとめでは次のステップとして紹介しますが、このワークショップの演習には含まれません。
- **Customize** — MCP サーバー、プラグイン、スキルなどを追加して Copilot app の機能を拡張します。Playwright MCP の設定で使います。
- **Chats** — 独自のブランチやワークスペースを必要としない、質問やブレインストーミング向けの簡易的な会話です。最初のチャットを開始すると表示されます。表示されない場合は **Sessions** の横にある **+** で最初のチャットを始めます。
- **Sessions** — リポジトリ フォルダーとエージェント セッションをまとめます。リポジトリ名 (例: `tailspin-toys`) を選ぶとそのリポジトリのビューを開き、配下のセッションを選ぶと会話に戻ります。セッションごとに分離されたワークスペースを使うため、変更を競合させず複数を同時に実行できます。最初のセッションは星評価を追加するレッスンで開始します。

ワークショップを進めながら、ワークスペースの機能を確認していきます。

> [!TIP]
> 操作方法がわからない場合や、実現できるかどうか不明な場合は、Copilot に質問すると案内してくれます。

### 用意されたバックログを確認する

ほとんどのプロジェクトにバックログがあり、Tailspin Toys にもあります。テンプレート作成時に用意された Issue を確認します。

1. サイドバーの **Sessions** で Tailspin Toys のリポジトリ フォルダーを選択します。配下の個別セッションではありません。次に、リポジトリ上部のナビゲーションで **Issues** を選択します。
2. Issue 番号を決めつけず、次のタイトルを探します。
   - Allow users to filter games by category and publisher
   - Update our repository coding standards
3. Issue を選択して詳細を読みます。Issue からエージェント セッションを開始できます。最初の簡単な変更を完了した後、絞り込み Issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加したリポジトリの項目だけが自動的に表示されます。他のリポジトリの作業項目も確認する場合は、そのリポジトリをアプリに追加してください。

## クイックチャットを試す

アプリ自体について質問すると、アプリの使い方に慣れることができます。**quick chat** はブランチや worktree を作成せずに質問やブレインストーミングを行えるため、その場限りの質問に適しています。

1. サイドバーの **Chats** の横にある **+** を選び、新しいチャットを開きます。**Chats** がまだ表示されない場合は、**Sessions** の横の **+** から最初のチャットを始めます。その後 **Chats** が表示されます。
2. セッションの仕組みをアプリに尋ねます。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが分離された独自の git worktree で動作するため、変更を競合させず複数のエージェントを並列に実行できます。会話は続けることも、新しいチャットを始めることもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。アプリへのサインイン、GitHub リポジトリからのプロジェクト追加、**Issues** でのバックログの確認、クイックチャットによる簡単な質問の方法を学びました。

次は[最初のエージェント セッションを開始し][next-lesson]、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app を使い始める][getting-started]
- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
