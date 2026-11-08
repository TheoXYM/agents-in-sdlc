---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーで絞り込み機能を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が利用できる自動化機能を追加する必要もあります。MCP サーバーを使うと、Copilot CLI に組み込まれている機能を拡張し、さらに多くのツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を学びます。
- Playwright MCP サーバーがまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させ、絞り込み機能を確認します。

## シナリオ

ユニットテストや E2E テストも重要ですが、UI の変更を検証するには、実際に操作する必要があります。ユーザーと同じように Web サイトを使えるよう Copilot を設定し、変更が期待どおりに動作する確信を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP によって、エージェントは最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

ツールやリソースには、AI エージェントと外部サービスの橋渡しをする MCP サーバーを通じてアクセスします。MCP サーバーごとに、エージェントが利用できるツールとリソースが異なります。

よく使われる MCP サーバーの例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

他にも多くの MCP サーバーがあります。GitHub が公開している [MCP レジストリ][mcp-registry]から、エコシステム内のサーバーを見つけたり、貢献したりできます。

> [!CAUTION]
> MCP サーバーは他の依存関係と同様に扱ってください。利用前にソース コードを確認し、公開元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、セットアップを飛ばして次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd>、次に <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認する

先ほどは手動で機能を確認しました。今度は Copilot から Playwright MCP サーバーを使って、同じように確認します。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は MCP サーバーを指定しなくても Copilot が適切なツールを判断します。ただし、使用するツールを指定すると結果の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザーで確認する手順を実行する様子を見ます。
3. レポートを読み、すべて期待どおりに動くか確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認しました。このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びました。
- 必要に応じて Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、絞り込み機能を確認しました。

次は、スキルとブラウザー ツールを組み合わせる専門的な役割として、[QA カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
