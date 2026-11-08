---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

ほとんどの組織は、技術的負債の問題を抱えています。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残っている機能要求などです。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行します。作業を終えると、人間の開発者がレビューできるドラフト PR を作成します。これにより、手間のかかる作業を委任して開発を加速し、より重要な作業に集中できます。

Copilot cloud agent を使って、次のことを学びます。

- コード生成環境をカスタマイズする。
- 操作を安全に実行する。
- 範囲が明確な Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初の Web サイトを作成した委託先は、ドキュメントを適切に整備しませんでした。まず、アプリケーションのすべてのエクスポート関数に TSDoc コメントを追加したいと考えています。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のタイトルを「関連ゲーム」として表示し、閲覧を続けられるようにします。現時点では、UX の受け入れテストに使える簡単な実装が必要です。現在は他に優先度の高い Issue があり、この作業は後回しになっています。

どちらも優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てれば、開発者は別の作業に集中し、後から Copilot の変更をレビューできます。Copilot はバックグラウンドで非同期に作業します。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。[GitHub Issue を Copilot に割り当てる][assign-issue]など、複数の方法でタスクを依頼できます。割り当てると、Copilot は進捗を追跡するドラフト プルリクエストを作成し、環境を準備してタスクに取り組みます。作業中または完了後にセッションを確認できます。提案された解決方法のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑さがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階を踏んで学び、試し、必要に応じて調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習でカスタム指示と Copilot への影響を確認しました。カスタム指示の演習をすでに行った場合は `.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリの指示ファイルを読み取り専用で確認して、後で効果を見比べられるようにします。

GitHub Web UI または起動中の Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` — 特に **Code standards** セクションの TypeScript 規約と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` frontmatter が `**/*.test.ts` に指示を適用することを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、生成された PR に TSDoc コメント、TypeScript 規約、コメント ヘッダーが反映されているか確認します。これらの指示ファイルによる影響は、後のレッスンで PR をレビューするときにも確認します。

## Copilot cloud agent 用の開発環境を設定する

コードを作成するときは、必要な環境やセットアップ スクリプトを用意します。Copilot にタスクを割り当てる場合も同様です。Copilot はソフトウェア エンジニアのように作業するためです。

Cloud agent は [GitHub Actions][github-actions] を作業環境として使います。`.github/workflows/copilot-setup-steps.yml` に設定した[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係をインストールできます。このラボでは、学習をスムーズに進めるため、ワークフローを事前に設定しました。単一の Astro アプリに必要な Node.js、プロジェクト依存関係、E2E テスト用のブラウザー、マイグレーションとシードが済んだ SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要な手順に最低限必要な権限を設定します。Copilot は自身の操作用トークンを受け取ります。
    permissions:
      # セットアップ中にリポジトリをクローンして依存関係をインストールする場合は、contents: read 権限が必要です。クローンしない場合は、セットアップ完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースをマイグレーションし、ビルドとテスト用のデータを投入します。
      - name: Set up the database
        run: npm run db:setup
```

これは他の GitHub ワークフローと同様ですが、重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは Copilot がプルリクエストで作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot が実行するのを待たずに、正常に動くかテストできます。

## ドキュメントを追加する

多くのプロジェクトでは、ドキュメントが古いか、まったくありません。このような技術的負債は放置されやすく、開発効率を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成が得意なため、cloud agent に依頼するのに適した作業です。後の演習で生成された変更をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの新しいプルリクエスト (PR) を開きます。一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が作成した todo リストが表示されます。

> [!NOTE]
> PR に Copilot の todo リストが表示されるまで数分かかる場合があります。Copilot は前述のワークフローを実行して環境を作り、プロジェクトを分析して、最適な方法を検討します。

13. todo リストと、Copilot が実行する予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** を選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示の更新を確認するため、ウィンドウの再読み込みが必要な場合があります。

16. ライブ セッションをスクロールして Copilot の作業を確認します。コードを調べて状態を把握し、計画を考え、コードを作成する様子が表示されます。

この作業には数分かかる場合があります。Copilot cloud agent の主な利点は、非同期でタスクを進められることです。別の作業に集中できるため、もう 1 つのタスクも cloud agent に割り当て、その間にアプリの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザイン チーム向けの関連ゲーム表示に長時間かからないとしても、その時間を他の作業に使えます。cloud agent に依頼しましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択します。
4. 空のテンプレートとして **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を開始したことを示す 👀 が Issue の最初のコメントに表示されます。

![Copilot が Issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューできます。次はカスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、Issue を割り当てて非同期に作業させられます。技術的負債の解消、新機能の作成、フレームワーク間のコード移行などに利用できます。

このレッスンでは、コード生成環境のカスタマイズ、安全な操作、範囲が明確な Issue の重要性、Issue の割り当てについて学びました。

Cloud agent が作業している間に、次はカスタム エージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで扱ったカスタム指示も適用されます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
