---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードで絞り込み機能を構築しました。プルリクエストを開く前に、ブラウザーで動作を確認します。自分でアプリを操作する代わりに **Playwright MCP サーバー** を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーで Copilot を拡張する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーで絞り込み機能を手動テストするよう Copilot に依頼します。
- 絞り込み作業のブランチを公開し、プルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードはエディターの外部に接続できると、さらに便利になります。Copilot は Model Context Protocol (MCP) を使います。これは、エージェントが外部のツールやサービスと通信するための標準的な方法です。

![エージェント モードの内部構造と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信する仕組みです。MCP を使うと、エージェントはリソースを通じて最新情報にアクセスしたり、ツールを使ってユーザーに代わる操作を実行したりできます。

ツールやリソースには、AI エージェントと外部サービスの橋渡しをする MCP サーバーを通じてアクセスします。MCP サーバーは既存 API や NPM パッケージなどのローカル ツールとの通信を管理します。各サーバーは異なるツールとリソースを提供します。

よく使われる MCP サーバーの例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などをエージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページの移動、フォームへの入力、ボタンの選択などをエージェントが実行できます。

他にも多くの MCP サーバーがあります。GitHub が公開している [MCP レジストリ](https://github.com/mcp)から、エコシステム内のサーバーを見つけたり、貢献したりできます。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーは他の依存関係と同様に扱ってください。利用前にソース コードをよく確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクト テンプレートに含まれているため、前の演習で Copilot はバックログの Issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルへのインストールは不要です。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot がブラウザーを操作でき、機能のテストに利用できます。

1. `.vscode/mcp.json` で `github` と並べて `playwright` を追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモート サーバーの `github` とは異なり、`playwright` はローカル サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` は、画面を表示せずにブラウザーを実行するオプションです。デスクトップ環境がない Codespace ではこれが必要です。

> [!NOTE]
> Tailspin Toys では E2E テストに Playwright を使っているため、必要なブラウザーはすでにインストールされていることが多いです。Copilot からブラウザーがないと報告された場合は、`npx playwright install chromium` を実行させてから再試行します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot がサーバーのツールを初めて使うときにサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` の上に **Start** が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. サーバーを信頼するか尋ねられたら、構成を確認して、起動を許可する場合は信頼を選択します。

手動での起動は省略可能です。実行しない場合は、次の手順で Copilot が初めてツールを使うときに VS Code が起動と信頼確認を行います。

## 絞り込み機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できる実際のブラウザーを提供します。エージェントはページを開き、移動し、フィルターを適用して、その結果を読み取って報告できます。会話を離れずに機能をすばやく確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。つまり、エージェントは支援技術と同様に、ラベル付きの構造化要素 (ボタン、リンク、リスト項目など) を基に判断します。簡単な機能確認が、アクセシビリティの基本確認にもなります。

サーバーを接続し、アプリを起動した状態で、次のプロンプトを Copilot に送ります。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーからブラウザーを起動し、手順に沿って確認し、結果を報告します。Issue に記載された受け入れ条件と結果を比べます。問題がある場合は追加で質問するか、PR を開く前にコードの修正を依頼します。

> [!NOTE]
> テストではアプリを `http://localhost:4321` で実行している必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が初めて Playwright MCP サーバーを使うとき、ブラウザーのダウンロードが必要になる場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行させて再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能の動作を確認できたら、チームのレビュー用にプルリクエスト (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミット メッセージを生成します。

   ![変更が表示された Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリに push します。

## プルリクエストを作成する

GitHub.com や GitHub コマンド ライン インターフェイス (CLI) などで PR を作成できます。ここでは GitHub Copilot に依頼します。関連する Issue を見つけ、PR と関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントのドロップダウンで **Agent** を選び、Copilot が GitHub ツールを使えるようにします。
3. 次のプロンプトを送信します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報の取得や操作を許可するため、必要に応じて **Continue** を選択します。GitHub ツールを初めて使うとき、認証を求められる場合があります。画面の指示に従います。
5. Copilot が Issue を検索し、正しい Issue を見つけて PR を作成する様子を確認します。
6. Copilot が表示したリンクを選択して PR を確認します。ただし、まだマージしないでください。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーで Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーで絞り込み機能を手動テストしました。
- ブランチを公開し、絞り込み作業の PR を作成しました。

MCP サーバーを使って機能をテストしましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューなどに使える[カスタム エージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
