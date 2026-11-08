---
title: "演習 2 - Copilot エージェント モードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

簡単な更新でも、通常は複数のファイル変更やテスト実行などが必要です。開発者は必要なファイルを探して変更し、テストを実行し、問題をデバッグして、変更漏れがあればさらに修正します。この作業は何度も続くことがあります。

ここで役立つのが Copilot エージェント モードです。

Copilot エージェント モードは IDE 内でより自律的に作業します。プロジェクト構成を調べ、必要なファイルを更新し、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェント モードを使って Web サイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途の機能です。エージェント モードは IDE 内で作業し、すばやいフィードバックと対話を可能にします。Cloud agent は Issue やプルリクエストを通じてやり取りし、チームの一員のように非同期で作業します。

この演習では、次のことを学びます。

- Copilot エージェント モードがプロジェクトを調べ、関連ファイルを見つけ、複数のファイルを変更する方法。
- UI とデータ層にまたがる新機能の実装方法。
- Copilot が生成した変更とテストを、コードベースにマージする前にレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリや発行元で絞り込めるようにします。前の演習で発行元のヘルパーを追加しました。今回は Copilot エージェント モードを使って、データ層、UI、テストの残りの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認して、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示します。Astro アプリはビルド時に静的 HTML を生成します。ページは frontmatter 内で Drizzle ORM を使ってローカル SQLite データベースを直接参照するため、独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` にあり、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行するため、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次のように実行します。

1. Codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 絞り込み作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら Astro サーバーのアドレス `http://localhost:4321` を選択し、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選ぶと、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Web サーバーを実行する Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: ボードゲームの一覧、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどを含む詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 手順を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なるため、必要なときに説明します。

Web サイトは動作しますが、さらに機能を追加したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを作成したときに Issue のバックログも自動作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから、変更を自律的に計画・実装し、ターミナル コマンドやツールを使える **Agent** を選択します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。手順の再現性を高めるため、作成時に使用した Claude Sonnet 4.5 を記載しています。LLM は確率的に動作するため、提案は異なる場合があります。これは通常の動作です。

> [!NOTE]
> 確率的な動作のため、Copilot が別の MCP コマンドを使用する場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送り、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue 一覧を確認します。

Copilot は、ユーザーに最も役立つと考えられる項目に優先順位も付けています。

## 指示ファイルを確認する

コード生成をエージェントに依頼する前に、作業のコンテキストを与える指示ファイルを確認します。ここでは、Web サイトへの機能追加方法を説明する [user interface (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加のガイダンスを確認します。これには次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイル形式に対応する指示へのリンク。

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。これにより、指示を再利用可能な小さなファイルに分け、例やテンプレートを示せます。

## 絞り込み機能を実装する

絞り込みを完成するには、少なくとも次の 3 つの変更が必要です。

- データ層 (`src/lib/`) の絞り込みロジックを追加または調整する。
- 絞り込み動作のテストを追加または更新する。
- ゲーム一覧ページに絞り込み UI を追加する。

コードベースにマージする前に、テストを実行して成功させる必要もあります。Copilot エージェント モードに作業を依頼しましょう。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルの選択例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルで **Claude Sonnet 4.5** が選ばれていることを確認します。
5. 関連するバックログ Issue の要件に基づいて機能を実装するよう、次のプロンプトを送ります。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、データ層ヘルパー、UI、テストなど関連ファイルを見つけて変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクト ファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、複雑さによっては数分かかります。作業中、コード エディターに **Keep** と **Undo** が表示されることがあります。Copilot の作業が完了すると、変更ごとに **Keep** または **Undo** を選べるので、作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![テスト実行の確認を求める Copilot Chat のダイアログを示すスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はエラーを検出しなくなるまでコード生成とテストを繰り返します。

   ![Copilot エージェント モードの完了した Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェント モードを使って Tailspin Toys に機能を追加しました。エージェント モードで UI とデータ層にまたがる機能を実装し、プロジェクトを調べて関連ファイルを見つけ、コードとテストを調整する方法を学びました。また、コードベースにマージする前に変更とテストをレビューしました。

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 発展演習 - ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションの実装を依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベスト プラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定するかなど、ページネーションの仕様を検討します。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に追加のコンテキストを与えて変更を依頼するなど、何度か反復する場合があります。これは Copilot を使う通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
